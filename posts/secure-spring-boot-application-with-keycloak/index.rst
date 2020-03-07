.. title: Secure spring boot application with keycloak
.. slug: secure-spring-boot-application-with-keycloak
.. date: 2018-11-30 18:19:12 UTC+07:00
.. tags: security, spring-boot, keycloak
.. category: 
.. link: 
.. description: configure keycloak for our application
.. type: text

In last blog post of this series we saw how we can configure keycloak for our application.
Now in this tutorial we
will see how we can use keycloak with spring boot.

**Prerequisite**

* Docker
  
* Keycloak

**Version**

- Spring Boo: 1.5.17.RELEASE

- Java: 1.8

- Keycloak: 4.0.5-Final


**Create sample spring boot application**
*******************************************

**Dependencies**

- Spring-boot-starter-web

- Keycloak-spring-boot-starter

**Create rest controller class**

{{%gist 70017889724bed1e3350b38a40d255ad %}}

**Secure App with keycloak**

Add maven dependency for keycloak and spring security - 

- spring-boot-starter-security

- keycloak-spring-boot-starter

Configure keycloak server url and realms details in application.properties file

{{%gist 4cd59c38757f8f6df9eff7bdf4ff16e4 %}}


**Configure keycloak security settings in the application**

Add the blow class to configure the keycloak

{{%gist 82b136433b67a825a323e10b08c82694 %}}

`KeycloakSecurityConfigurer.class` extends `KeycloakWebSecurityConfigurerAdapter.class` that provide convenient base class for creating a `WebSecurityConfigurer` instance secured by Keycloak.

`GrantedAuthoritiesMapper` is mapping interface which use to convert case of the role used in the keycloak from lower case to uppercase.

`KeycloakAuthenticationProvider` perform authentication process.

`NullAuthenticatedSessionStrategy` since we are using rest full service so we can provide null authenticated session strategy.

`KeycloakConfigResolver` use to tell keycloak to use spring boot configuration. Instead
use the configuration from the spring boot configuration resolver.

`keycloakAuthenticationProcessingFilterRegistrationBean`, `keycloakPreAuthActionsFilterRegistrationBean` are used avoid re-registration of the filter.

**Running application**
*************************

start the application using

`mvn spring-boot:run`


`Call the admin api without security token`

.. thumbnail:: /images/20181130/img1.png
   :height: 600px
   :width: 1200 px
   :scale: 50 %
   :alt: alternate text

`Get access token for admin role`

.. thumbnail:: /images/20181130/img2.png
   :height: 600px
   :width: 1200 px
   :scale: 50 %
   :alt: alternate text

`Access admin api with access token`

.. thumbnail:: /images/20181130/img3.png
   :height: 600px
   :width: 1200 px
   :scale: 50 %
   :alt: alternate text

`Access user api with access token will give error because user role required for access user service.`

.. thumbnail:: /images/20181130/img4.png
   :height: 600px
   :width: 1200 px
   :scale: 50 %
   :alt: alternate text

`Get access token for user role`

.. thumbnail:: /images/20181130/img5.png
   :height: 600px
   :width: 1200 px
   :scale: 50 %
   :alt: alternate text

`Access user service`

.. thumbnail:: /images/20181130/img6.png
   :height: 600px
   :width: 1200 px
   :scale: 50 %
   :alt: alternate text

You can get the source code from `Bitbucket <http://bit.ly/2SkdMwL>`_
