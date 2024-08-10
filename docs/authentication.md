# Authentication

- [Basic Auth](#built-in-auth)
  - [Setting Up Authentication](#setting-up-authentication)
  - [Hash Password](#hash-password)
  - [Logging In and Out](#logging-in-and-out)
  - [Guest Access](#enabling-guest-access)
  - [Per-User Access](#granular-access)
  - [Using Environment Variables for Passwords](#using-environment-variables-for-passwords)
  - [Adding HTTP Auth to Configuration](#adding-http-auth-to-configuration)
  - [Security Considerations](#security)
- [HTTP Auth](#http-auth)
- [Keycloak Auth](#keycloak)
  - [Deploying Keycloak](#1-deploy-keycloak)
  - [Setting up Keycloak](#2-setup-keycloak-users)
  - [Configuring Homeview for Keycloak](#3-enable-keycloak-in-homeview-config-file)
  - [Toubleshooting Keycloak](#troubleshooting-keycloak)
- [Alternative Authentication Methods](#alternative-authentication-methods)
  - [VPN](#vpn)
  - [IP-Based Access](#ip-based-access)
  - [Web Server Authentication](#web-server-authentication)
  - [OAuth Services](#oauth-services)
  - [Auth on Cloud Hosting Services](#static-site-hosting-providers)


> [!IMPORTANT]
> Homeview's built-in auth is not indented to protect a publicly hosted instance against unauthorized access. Instead you should use an auth provider compatible with your reverse proxy, or access Homeview via your VPN, or implement your own SSO logic. 
>
> In cases where Homeview is only accessibly within your home network, and you just want to add a login page, then the built-in auth may be sufficient, but keep in mind that configuration can still be accessed.

## Built-In Auth

Homeview has a basic login page included, and frontend authentication. You can enable this by adding users to the `auth` section under `appConfig` in your `conf.yml`. If this section is not specified, then no authentication will be required to access the app, and the homepage will resolve to your dashboard.

> [!NOTE]
> Since the auth is initiated in the main app entry point (for security), a rebuild is required to apply changes to the auth configuration.
> You can trigger a rebuild through the UI, under Config --> Rebuild, or by running `yarn build` in the root directory.


### Setting Up Authentication

The `auth` property takes an array of users. Each user needs to include a username, hash and optional user type (`admin` or `normal`). The hash property is a [SHA-256 Hash](https://en.wikipedia.org/wiki/SHA-2) of your desired password.

For example:

```yaml
appConfig:
  auth:
    users:
    - user: khulnasoft
      hash: 4D1E58C90B3B94BCAD9848ECCACD6D2A8C9FBC5CA913304BBA5CDEAB36FEEFA3
      type: admin
    - user: bob
      hash: 5E884898DA28047151D0E56F8DC6292773603D0D6AABBDD62A11EF721D1542D8
```

### Hash Password

Homeview uses [SHA-256 Hash](https://en.wikipedia.org/wiki/Sha-256), a 64-character string, which you can generate using an online tool, such as [this one](https://passwordsgenerator.net/sha256-hash-generator/) or [CyberChef](https://gchq.github.io/CyberChef/) (which can be self-hosted/ ran locally).

A hash is a one-way cryptographic function, meaning that it is easy to generate a hash for a given password, but very hard to determine the original password for a given hash. This means, that so long as your password is long, strong and unique, it is safe to store its hash in the clear. Having said that, you should never reuse passwords, hashes can be cracked by iterating over known password lists, generating a hash of each.

### Logging In and Out

Once authentication is enabled, so long as there is no valid token in cookie storage, the application will redirect the user to the login page. When the user enters credentials in the login page, they will be checked, and if valid, then a token will be generated, and they can be redirected to the home page. If credentials are invalid, then an error message will be shown, and they will remain on the login page. Once in the application, to log out: the user can click the logout button (in the top-right), which will clear cookie storage, causing them to be redirected back to the login page.

### Enabling Guest Access

With authentication set up, by default no access is allowed to your dashboard without first logging in with valid credentials. Guest mode can be enabled to allow for read-only access to a secured dashboard by any user, without the need to log in. A guest user cannot write any changes to the config file, but can apply modifications locally (stored in their browser). You can enable guest access, by setting `appConfig.auth.enableGuestAccess: true`.

### Granular Access

You can use the following properties to make certain pages, sections or items only visible to some users, or hide pages, sections and items from guests.

- `hideForUsers` - Page, Section or Item will be visible to all users, except for those specified in this list
- `showForUsers` - Page, Section or Item will be hidden from all users, except for those specified in this list
- `hideForGuests` - Page, Section or Item will be visible for logged in users, but not for guests

For Example:
```yaml
pages:
  - name: Home Lab
    path: home-lab.yml
    displayData:
      showForUsers: [admin]
  - name: Intranet
    path: intranet.yml
    displayData:
      hideForGuests: true
      hideForUsers: [khulnasoft, bob]
```    

```yaml
- name: Code Analysis & Monitoring
  icon: fas fa-code
  displayData:
    cols: 2
    hideForUsers: [khulnasoft, bob]
  items:
    ...
```

```yaml
- name: Deployment Pipelines
  icon: fas fa-rocket
  displayData:
    hideForGuests: true
  items:
    - title: Hide Me
      displayData:
        hideForUsers: [khulnasoft, bob]
```

### Permissions

Any user who is not an admin (with `type: admin`) will not be able to write changes to disk.

You can also prevent any user from writing changes to disk, using `preventWriteToDisk`. Or prevent any changes from being saved locally in browser storage, using `preventLocalSave`. Both properties can be found under [`appConfig`](./docs/configuring.md#appconfig-optional).

To disable all UI config features, including View Config, set `disableConfiguration`. Alternatively you can disable UI config features for all non admin users by setting `disableConfigurationForNonAdmin` to true.

### Using Environment Variables for Passwords

If you don't want to hash your password, you can instead leave out the `hash` attribute, and replace it with `password` which should have the value of an environmental variable name you wish to use.

Note that env var must begin with `VUE_APP_`, and you must set this variable before building the app.

For example:

```yaml
  auth:
    users:
    - user: bob
      password: VUE_APP_BOB
```

Just be sure to set `VUE_APP_BOB='my super secret password'` before build-time.

### Adding HTTP Auth to Configuration

If you'd also like to prevent direct visit access to your configuration file, you can set the `ENABLE_HTTP_AUTH` environmental variable.

### Security

With basic auth, all logic is happening on the client-side, which could mean a skilled user could manipulate the code to view parts of your configuration, including the hash. If the SHA-256 hash is of a common password, it may be possible to determine it, using a lookup table, in order to find the original password. Which can be used to manually generate the auth token, that can then be inserted into session storage, to become a valid logged in user. Therefore, you should always use a long, strong and unique password, and if you instance contains security-critical info and/ or is exposed directly to the internet, and alternative authentication method may be better. The purpose of the login page is merely to prevent immediate unauthorized access to your homepage.

**[⬆️ Back to Top](#authentication)**

---

## HTTP Auth

If you'd like to protect all your config files from direct access, you can set the `BASIC_AUTH_USERNAME` and `BASIC_AUTH_PASSWORD` environmental variables. You'll then be prompted to enter these credentials when visiting Homeview.

Then, if you'd like your frontend to automatically log you in, without prompting you for credentials (insecure, so only use on a trusted environment), then also specify `VUE_APP_BASIC_AUTH_USERNAME` and `VUE_APP_BASIC_AUTH_PASSWORD`. This is useful for when you're hosting Homeview on a private server, and just want to use auth for user management and to prevent direct access to your config files, while still allowing the frontend to access them. Note that a rebuild is required for these changes to take effect.

**[⬆️ Back to Top](#authentication)**

---

## Keycloak

Homeview also supports using a [Keycloak](https://www.keycloak.org/) authentication server. The setup for this is a bit more involved, but it gives you greater security overall, useful for if your instance is exposed to the internet.

[Keycloak](https://www.keycloak.org/about.html) is a Java-based [open source](https://github.com/keycloak/keycloak), high-performance, secure authentication system, supported by [RedHat](https://www.redhat.com/en). It is easy to setup ([with Docker](https://quay.io/repository/keycloak/keycloak)), and enables you to secure multiple self-hosted applications with single-sign-on using standard protocols (OpenID Connect, OAuth 2.0, SAML 2.0 and social login). It's also very customizable, you can write or use custom [themes](https://wjw465150.gitbooks.io/keycloak-documentation/content/server_development/topics/themes.html), [plugins](https://www.keycloak.org/extensions.html), [password policies](https://wjw465150.gitbooks.io/keycloak-documentation/content/server_admin/topics/authentication/password-policies.html) and more.
The following guide will walk you through setting up Keycloak with Homeview. If you already have a Keycloak instance configured, then skip to Step 3.

### 1. Deploy Keycloak

First thing to do is to spin up a new instance of Keycloak. You will need [Docker installed](https://docs.docker.com/engine/install/), and can then choose a tag, and pull the container from [quay.io/keycloak/keycloak](https://quay.io/repository/keycloak/keycloak)

Use the following run command, replacing the attributes (default credentials, port and name), or incorporate this into your docker-compose file.

```bash
docker run -d \
  -p 8081:8080 \
  --name auth-server \
  -e KEYCLOAK_USER=admin \
  -e KEYCLOAK_PASSWORD=admin \
  quay.io/keycloak/keycloak:15.0.2
```

If you need to pull from DockerHub, a non-official image is available [here](https://registry.hub.docker.com/r/jboss/keycloak). Or if you would prefer not to use Docker, you can also directly install Keycloak from source, following [this guide](https://www.keycloak.org/docs/latest/getting_started/index.html).

You should now be able to access the Keycloak web interface, using the port specified above (e.g. `http://127.0.0.1:8081`), login with the default credentials, and when prompted create a new password.

### 2. Setup Keycloak Users

Before we can use Keycloak, we must first set it up with some users. Keycloak uses Realms (similar to tenants) to create isolated groups of users. You must create a Realm before you will be able to add your first user.

1. Head over to the admin console
2. In the top-left corner there is a dropdown called 'Master', hover over it and then click 'Add Realm'
3. Give your realm a name, and hit 'Create'

You can now create your first user.

1. In the left-hand menu, click 'Users', then 'Add User'
2. Fill in the form, including username and hit 'Save'
3. Under the 'Credentials' tab, give the new user an initial password. They will be prompted to change this after first login

The last thing we need to do in the Keycloak admin console is to create a new client

1. Within your new realm, navigate to 'Clients' on the left-hand side, then click 'Create' in the top-right
2. Choose a 'Client ID', set 'Client Protocol' to 'openid-connect', and for 'Valid Redirect URIs' put a URL pattern to where you're hosting Homeview (if you're just testing locally, then * is fine), and do the same for the 'Web Origins' field
3. Make note of your client-id, and click 'Save'

### 3. Enable Keycloak in Homeview Config File

Now that your Keycloak instance is up and running, all that's left to do is to configure Homeview to use it. Under `appConfig`, set `auth.enableKeycloak: true`, then fill in the details in `auth.keycloak`, including: `serverUrl` - the URL where your Keycloak instance is hosted, `realm` - the name you gave your Realm, and `clientId` - the Client ID you chose.
For example:

```yaml
appConfig:
  ...
  auth:
    enableKeycloak: true
    keycloak:
      serverUrl: 'http://localhost:8081'
      realm: 'khulnasoft-homelab'
      clientId: 'homeview'
```

Note that if you are using Keycloak V 17 or older, you will also need to set `legacySupport: true` (also under `appConfig.auth.keycloak`). This is because the API endpoint was updated in later versions.

If you use Keycloak with an external Identity Provier, you can set the `idpHint: 'alias-of-kc-idp'` option to allow the IdP Hint to be passed to Keycloak. This will cause Keycloak to skip its login page and redirect the user directly to the specified IdP's login page. Set to the value of the 'Alias' field of the desired IdP as defined in Keycloak under 'Identity Providers'.

### 4. Add groups and roles (Optional)

Keycloak allows you to assign users roles and groups. You can use these values to configure who can access various sections or items in Homeview.
Keycloak server administration and configuration is a deep topic; please refer to the [server admin guide](https://www.keycloak.org/docs/latest/server_admin/index.html#assigning-permissions-and-access-using-roles-and-groups) to see details about creating and assigning roles and groups.
Once you have groups or roles assigned to users you can configure access under each section or item `displayData.showForKeycloakUser` and `displayData.hideForKeycloakUser`.
Both show and hide configurations accept a list of `groups` and `roles` that limit access. If a users data matches one or more items in these lists they will be allowed or excluded as defined.

```yaml
sections:
  - name: DeveloperResources
    displayData:
      showForKeycloakUsers:
        roles: ['canViewDevResources']
      hideForKeycloakUsers:
        groups: ['ProductTeam']
    items:
      - title: Not Visible for developers
        displayData:
          hideForKeycloakUsers:
            groups: ['DevelopmentTeam']
```

Depending on how you're hosting Homeview and Keycloak, you may also need to set some HTTP headers, to prevent a CORS error. This would typically be the `Access-Control-Allow-Origin [URL-of Homeview]` on your Keycloak instance. See the [Setting Headers](https://github.com/KhulnaSoft/homeview/blob/master/docs/management.md#setting-headers) guide in the management docs for more info.

Your app is now secured :) When you load Homeview, it will redirect to your Keycloak login page, and any user without valid credentials will be prevented from accessing your dashboard.

From within the Keycloak console, you can then configure things like time-outs, password policies, etc. You can also backup your full Keycloak config, and it is recommended to do this, along with your Homeview config. You can spin up both Homeview and Keycloak simultaneously and restore both applications configs using a `docker-compose.yml` file, and this is recommended.

---

### Troubleshooting Keycloak

If you encounter issues with your Keycloak setup, follow these steps to troubleshoot and resolve common problems.

1. Client Authentication Issue
Problem: Redirect loop, if client authentication is enabled.
Solution: Switch off "client authentication" in "TC clients" -> "Advanced" settings.

2. Double URL
Problem: If you get redirected to "https://homeview.my.domain/#iss=https://keycloak.my.domain/realms/my-realm"
Solution: Make sure to turn on "Exclude Issuer From Authentication Response" in "TC clients" -> "Advanced" -> "OpenID Connect Compatibility Modes"

3. Problems with mutiple Homeview Pages
Problem: Refreshing or logging out of homeview results in an "invalid_redirect_uri" error.
Solution: In "TC clients" -> "Access settings" -> "Root URL" https://homeview.my.domain/, valid redirect URIs must be /*

---

## OIDC

Homeview also supports using a general [OIDC compatible](https://openid.net/connect/) authentication server. In order to use it, the authentication section needs to be configured:

```yaml
appConfig:
  auth:
    enableOidc: true
    oidc:
      clientId: [registered client id]
      endpoint: [OIDC endpoint]
```

Because Homeview is a SPA, a [public client](https://datatracker.ietf.org/doc/html/rfc6749#section-2.1) registration with PKCE is needed.

An example for Authelia is shared below, but other OIDC systems can be used:

```yaml
identity_providers:
  oidc:
    clients:
      - client_id: homeview
        client_name: homeview
        public: true
        authorization_policy: 'one_factor'
        require_pkce: true
        pkce_challenge_method: 'S256'
        redirect_uris:
          - https://homeview.local # should point to your homeview endpoint
        grant_types:
          - authorization_code
        scopes:
          - 'openid'
          - 'profile'
          - 'roles'
          - 'email'
          - 'groups'
```

Groups and roles will be populated and available for controlling display similar to [Keycloak](#Keycloak) abvoe.

---

## Alternative Authentication Methods

If you are self-hosting Homeview, and require secure authentication to prevent unauthorized access, then you can either use Keycloak, or one of the following options:

- [Authentication Server](#authentication-server) - Put Homeview behind a self-hosted auth server
- [VPN](#vpn) - Use a VPN to tunnel into the network where Homeview is running
- [IP-Based Access](#ip-based-access) - Disallow access from all IP addresses, except your own
- [Web Server Authentication](#web-server-authentication) - Enable user control within your web server or proxy
- [OAuth Services](#oauth-services) - Implement a user management system using a cloud provider
- [Password Protection (for cloud providers)](#static-site-hosting-providers) - Enable password-protection on your site

### Authentication Server

#### Authelia

[Authelia](https://www.authelia.com/) is an open-source full-featured authentication server, which can be self-hosted and either on bare metal, in a Docker container or in a Kubernetes cluster. It allows for fine-grained access control rules based on IP, path, users etc, and supports 2FA, simple password access or bypass policies for your domains.

- `git clone https://github.com/authelia/authelia.git`
- `cd authelia/examples/compose/lite`
- Modify the `users_database.yml` the default username and password is authelia
- Modify the `configuration.yml` and `docker-compose.yml` with your respective domains and secrets
- `docker-compose up -d`

For more information, see the [Authelia docs](https://www.authelia.com/docs/)

### VPN

A catch-all solution to accessing services running from your home network remotely is to use a VPN. It means you do not need to worry about implementing complex authentication rules, or trusting the login implementation of individual applications. However it can be inconvenient to use on a day-to-day basis, and some public and corporate WiFi block VPN connections. Two popular VPN protocols are [OpenVPN](https://openvpn.net/) and [WireGuard](https://www.wireguard.com/)

### IP-Based Access

If you have a static IP or use a VPN to access your running services, then you can use conditional access to block access to Homeview from everyone except users of your pre-defined IP address. This feature is offered by most cloud providers, and supported by most web servers.

#### Apache

In Apache, this is configured in your `.htaccess` file in Homeview's root folder, and should look something like:

```text
Order Deny,Allow
Deny from all
Allow from [your-ip]
```

#### NGINX

In NGINX you can specify [control access](https://docs.nginx.com/nginx/admin-guide/security-controls/controlling-access-proxied-http/) rules for a given site in your `nginx.conf` or hosts file. For example:

```text
server {
	listen 8080;
	server_name www.homeview.example.com;
	location / {
		root /path/to/homeview/;
		passenger_enabled on;
		allow [your-ip];
		deny all;
    }
  }
```

#### Caddy

In Caddy, [Request Matchers](https://caddyserver.com/docs/caddyfile/matchers) can be used to filter requests

```text
homeview.site {
	@public_networks not remote_ip [your-ip]
	respond @public_networks "Access denied" 403
}
```

### Web Server Authentication

Most web servers make password protecting certain apps very easy. Note that you should also set up HTTPS and have a valid certificate in order for this to be secure.

#### Apache

First crate a `.htaccess` file in Homeview's route directory. Specify the auth type and path to where you want to store the password file (usually the same folder). For example:

```text
AuthType Basic
AuthName "Please Sign into Homeview"
AuthUserFile /path/homeview/.htpasswd
require valid-user
```

Then create a `.htpasswd` file in the same directory. List users and their hashed passwords here, with one user on each line, and a colon between username and password (e.g. `[username]:[hashed-password]`). You will need to generate an MD5 hash of your desired password, this can be done with an [online tool](https://www.web2generators.com/apache-tools/htpasswd-generator).  Your file will look something like:

```text
khulnasoft:$apr1$jv0spemw$RzOX5/GgY69JMkgV6u16l0
```

#### NGINX

NGINX has an [authentication module](https://nginx.org/en/docs/http/ngx_http_auth_basic_module.html) which can be used to add passwords to given sites, and is fairly simple to set up. Similar to above, you will need to create a `.htpasswd` file. Then just enable auth and specify the path to that file, for example:

```text
location / {
  auth_basic "closed site";
  auth_basic_user_file conf/htpasswd;
}
```

#### Caddy

Caddy has a [basic-auth](https://caddyserver.com/docs/caddyfile/directives/basicauth) directive, where you specify a username and hash. The password hash needs to be base-64 encoded, the [`caddy hash-password`](https://caddyserver.com/docs/command-line#caddy-hash-password) command can help with this. For example:

```text
basicauth /secret/* {
	khulnasoft JDJhJDEwJEVCNmdaNEg2Ti5iejRMYkF3MFZhZ3VtV3E1SzBWZEZ5Q3VWc0tzOEJwZE9TaFlZdEVkZDhX
}
```

For more info about implementing a single sign on for all your apps with Caddy, see [this tutorial](https://joshstrange.com/securing-your-self-hosted-apps-with-single-signon/)

#### Lighttpd

You can use the [mod_auth](https://doc.lighttpd.net/lighttpd2/mod_auth.html) module to secure your site with Lighttpd. Like with Apache, you need to first create a password file listing your usernames and hashed passwords, but in Lighttpd, it's usually called `.lighttpdpassword`.

Then in your `lighttpd.conf` file (usually in the `/etc/lighttpd/` directory), load in the mod_auth module, and configure it's directives. For example:

```text
server.modules += ( "mod_auth" )
auth.debug = 2
auth.backend = "plain"
auth.backend.plain.userfile = "/home/lighttpd/.lighttpdpassword"

$HTTP["host"] == "homeview.my-domain.net" {
  server.document-root = "/home/lighttpd/homeview.my-domain.net/http"
  server.errorlog = "/var/log/lighttpd/homeview.my-domain.net/error.log"
  accesslog.filename = "/var/log/lighttpd/homeview.my-domain.net/access.log"
  auth.require = (
    "/docs/" => (
      "method" => "basic",
      "realm" => "Password protected area",
      "require" => "user=khulnasoft"
    )
  )
}
```

Restart your web server for changes to take effect.

### OAuth Services

There are also authentication services, such as [Ory.sh](https://www.ory.sh/), [Okta](https://developer.okta.com/), [Auth0](https://auth0.com/), [Firebase](https://firebase.google.com/docs/auth/). Implementing one of these solutions would involve some changes to the [`Auth.js`](https://github.com/KhulnaSoft/homeview/blob/master/src/utils/Auth.js) file, but should be fairly straightforward.

### Static Site Hosting Providers

If you are hosting Homeview on a cloud platform, you will probably find that it has built-in support for password protected access to web apps. For more info, see the relevant docs for your provider, for example: [Netlify Password Protection](https://docs.netlify.com/visitor-access/password-protection/), [Cloudflare Access](https://www.cloudflare.com/teams/access/), [AWS Cognito](https://aws.amazon.com/cognito/), [Azure Authentication](https://docs.microsoft.com/en-us/azure/app-service/scenario-secure-app-authentication-app-service) and [Vercel Password Protection](https://vercel.com/docs/platform/projects#password-protection).

**[⬆️ Back to Top](#authentication)**
