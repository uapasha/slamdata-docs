.. figure:: /images/white-logo.png
   :alt: SlamData Logo

Reference - SlamData Advanced API
=================================

Configure
---------

The SlamData  configuration file is extended for SSL by introducing the ``ssl``
JSON pair to the ``server`` section and for authentication by introducing the
``authentication`` section as follows:

.. code:: json

    {
      "server": {
        "port": 8080,
        "ssl": {
          "enabled": true,
          "port": 9090,
          "cert": "<base64 encoded pkcs12 cert file>"
        }
      },

      "authentication": {
        "openid_providers": [
          {
            "display_name": "Google",
            "issuer": "https://accounts.google.com",
            "client_id": "sdf987wetlkdflkj"
          },
          {
            "display_name": "Our Company OP",
            "client_id": "123455976",
            "openid_configuration": {
              "issuer": "https://op.ourcompany.com",
              "authorization_endpoint": "https://op.ourcompany.com/authorize",
              "token_endpoint": "https://op.ourcompany.com/token",
              "userinfo_endpoint": "https://op.ourcompany.com/userinfo",
              "jwks": [
                {
                  "kty": "RSA",
                  "kid": "1234",
                  "alg": "RS256",
                  "use": "sig",
                  "n": "2354098udw...2957835lkj"
                },
                {
                  "kty": "RSA",
                  "kid": "5678",
                  "alg": "RS256",
                  "use": "sig",
                  "n": "skljhdfiugy...39587dlkjsd"
                }
              ]
            }
          }
        ]
      },

      "mountings": {
        "/": {
          "mongodb": {
            "connectionUri": "mongodb://<user>:<pass>@<host>:<port>/<dbname>"
          }
        }
      },

      "authorization": {
        "tokens": {
          "99C8B-append-us": [
            { "operation": "Add", "resource": "/us", "type": "Structural" }
          ],
          "762BF-manage-zips": [
            { "operation": "Add", "resource": "/ca/zips", "type": "Content" },
            { "operation": "Read", "resource": "/ca/zips", "type": "Content" },
            { "operation": "Delete", "resource": "/ca/zips", "type": "Content" }
          ],
          "C24AC-delete-mounts": [
            { "operation": "Delete", "resource": "/", "type": "Mount" }
          ]
        },
        "users": {
          "alice@example.com": [
            "99C8B-append-us"
          ],
          "bob@example.com": [
            "99C8B-append-us",
            "762BF-manage-zips"
          ],
          "chuck@example.com": []
        },
        "anonymousUser": [
          "762BF-manage-zips"
        ]
      },

      "auditing": {
        "log_file": "/mongodb/logs/quasar-audit-log"
      }
    }

SSL
~~~

If ``cert`` is absent from the config a self-signed certificate will be
generated.

When SSL is enabled, the server accepts SSL connections on the
configured SSL port and accepts non-ssl connections, restricted to
localhost, on the configured server port.

Generate a PKCS#12 file for use with the cert endpoint from cert and
private key PEM files:

::

    openssl pkcs12 -export -in cert.pem -inkey privatekey.pem -passout pass: -out cert-privatekey-pair.p12

Authentication
~~~~~~~~~~~~~~

Configuration for authentication consists of specifying the list of
OpenID Providers (OP) and Client Identifiers to accept ID Tokens from. A
display name is also required and will be used to identify the provider
to a user when displaying multiple provider options in a list.

The amount of provider configuration required depends on whether the OP
supports
`discovery <http://openid.net/specs/openid-connect-discovery-1_0.html>`__.
If an OP does support discovery, then only the display name, issuer and
client identifier need be provided (see the "Google" configuration in
the above example) as the rest of the pertinent information can be
obtained via the discovery protocol. If an OP does not support discovery
(or one does not wish to use discovery) then all the necessary details
for the OP must be provided (see the "Our Company OP" configuration in
the above example), including the signing keys necessary to validate ID
Tokens (in
`JWK <https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41>`__
format).

Authorization
~~~~~~~~~~~~~

Configuration for authorization consists of defining permission tokens
and assigning sets of them to users as well as specifying the set of
tokens that should be included for every user of the system via the
``anonymousUser``. Users are identified via the e-mail address obtained
from the OIDC ID Token used during authentication.

*Note*: Care should be taken to ensure permission tokens are not easily
guessable as providing one via the ``X-Extra-Permissions`` header grants
the associated capabilities irrespective of authentication status.

Auditing
~~~~~~~~

Optionally you may include an auditing section in your config file which
specifies a file where to log all filesystem operations for auditing
purposes.

Authentication
--------------

SlamData Advanced adds support for authenticated requests via the `OpenID
Connect <http://openid.net/connect/>`__ protocol. A request to any
Quasar or SlamData Advanced API may be authenticated. If no credentials
are included in a request, it is considered unauthenticated (or
"anonymous") and may fail if the system is not configured to allow
anonymous access for the given request.

Making an Authenticated Request
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To make an authenticated request, clients first need to ensure their
OpenID Provider (OP) has been configured in SlamData Advanced along with
the "Client Identifier" (CID) issued to the client by the OP, this
allows the SlamData Advanced administrator to specify which clients are
permitted to access SlamData Advanced. If an ID Token is received from a
known provider but with an *unknown* CID, it will be rejected outright.

Next the client should obtain the list of known providers from the
``/security/oidc/providers`` endpoint (see details on this endpoint
below) and authenticate the user against one of them, obtaining an `ID
Token <http://openid.net/specs/openid-connect-core-1_0.html#IDToken>`__.
The ID Token MUST be requested using at least the ``openid`` and
``email`` scopes and their claims must be included in the ID Token.

Once in possession of a valid ID Token, the client includes it,
verbatim, in the request to SlamData Advanced via the ``Authorization``
header as a `bearer
token <http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html>`__
using the ``Bearer`` scheme.

If a request includes valid authentication and the identified subject is
not permitted to perform the requested action per the authorization
policy, a ``403 Forbidden`` response will be returned. If, however, a
request which does not include any authentication information is denied
due to the authorization policy a ``401 Unauthorized`` response will be
returned to indicate that repeating the request with authentication may
allow it to succeed.

Authorization
-------------

SlamData Advanced adds support for authorization of service requests.
Permissions for a request are derived from the union of permission
tokens provided in the ``X-Extra-Permissions`` header and those
configured for the authenticated user and anonymous user. Permissions
are defined as an operation, its type, and a filesystem resource path. A
permission token grants a set of permissions.

The available operations and types are as follows:

| Type: Content, Structural, Mount
| Operation: Add, Read, Delete, Modify

+----------+------------------------+---------------------------+-----------------------+
|          | Content                | Structural                | Mount                 |
+==========+========================+===========================+=======================+
| Add      | append to file         | create resource           | create mount          |
+----------+------------------------+---------------------------+-----------------------+
| Read     | read file contents     | list directory            | retrieve mount info   |
+----------+------------------------+---------------------------+-----------------------+
| Delete   | delete file contents   | delete resource           | remove mount          |
+----------+------------------------+---------------------------+-----------------------+
| Modify   | modify file contents   | rename or move resource   | N/A                   |
+----------+------------------------+---------------------------+-----------------------+

A permission on a parent resource is sufficient to authorize an action
on a resource granted the nature and type of the operation are the same.

A ``403 Forbidden`` is returned by the server when a request does not
have sufficient permissions to perform the associated actions.

| The ``X-Extra-Permissions`` header is formatted as follows:
| ``X-Extra-Permissions: [token1],[token2]``

Known vulnerabilities (*planned to be addressed in a future release*)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  MongoDB generates temp files when performing certain kinds of
   queries. These temp files can potentially contain sensitive data and
   could potentially be accessible to users of the system if they happen
   to have access to directories where the temp files are generated.
   Unfortunately, the location of these files is implementation
   specific, so it is hard to predict where they will be created.
-  Similarly atomic writes are implemented using temporary files and
   therefore can be created anywhere in the filesystem. SlamData uses
   PUT to copy notebooks which in turn use atomic writes.

Auditing
--------

When a log file is specified in the configuration file, all filesystem
operations will be logged to that file. SlamData logs the operations as
data in the filesystem where the path is located. This means that it is
then possible to use SlamData itself to analyze the log data.

Additional APIs
---------------

Security API
~~~~~~~~~~~~

**NB: The ``/security`` endpoints are a WIP and are only stubbed out at
the moment**

Actions and permissions are central concepts to the security api. An
action is any operation a subject can perform on a given resource in the
system. A permission represents the capability of a subject (group,
user, token) in the system to perform a given action. All permissions
have a lineage which represents by which authority a permission was
granted to a subject. Any subject in the system has the authority to
grant a new permission which is a subset of one of their own
permissions. This new permission is said to have been derived from the
relevant permission(s) of the grantor and that/those relevant
permission(s) are said to be the parent(s) of that permission.

Permissions can be revoked. If a permission is revoked, that permission
as well as all permissions derived from it become invalid and can no
longer be used to perform operations in the system. It is possible
however for a permission to have been derived from more than one
permission. In such a case, revoking only one of its parents will not
lead to the permission becoming invalid. It will only become invalid
once all its parents have been revoked.

Actions and permissions are found throughout the following api endpoints
and are represented as follows in ``json``:

Action:

.. code:: json

    {
      "operation": "ADD|READ|MODIFY|DELETE",
      "resource": "<filesystem_path>|<group_path>",
      "resourceType": "Structural|Content",
    }

Permission:

.. code:: json

    {
      "id": "<permission_id>",
      "action": {
        "operation": "ADD|READ|MODIFY|DELETE",
        "resource": "<filesystem_path>|<group_path>",
        "resourceType": "Structural|Content",
      },
      "grantedTo": "<user_id>|<group_path>|<token_id>",
      "grantedBy": ["<user_id>", "<group_path>", "<token_id>", "..."]
    }

-  ``<filesystem_path>`` is a path in the SlamData virtual filesystem such
   as ``data:/foo/bar`` for a file and ``data:/foo/bar/`` for a
   directory
-  ``<group_path>`` is a path uniquely identifying a group and its
   location in the group hierarchy such as
   ``group:/engineering/backend``
-  ``<grantedBy>`` The sources of authority by which this permission was
   granted. In reality, only permissions themselves grant permission to
   grant permissions to others. Here we are simply surfacing the subject
   which possess the permission by which this permission was granted.
-  ``<user_id>`` is an email prefixed with the "user" string such as
   ``user:bob@example.com``
-  ``<token_id>`` is a string identifier prefixed by the "token" string
   such as ``token:786549382``
-  ``<permission_id>`` is an identifier

In the following api endpoints descriptions, "your permissions" refers
to the set of permissions associated with the request. In the case of an
authenticated user this means all permissions directly associated with
that user as well as all groups that user is a explicitly or implicitly
a part of. Additionally, any permission associated with tokens present
in the request headers are added to the permissions associated with the
request.

Whenever no return body is specified, a response with a 2XX status can
be expected along with an empty body.

In any of the following endpoints, if the request does not "carry"
sufficient permissions to satisfy the requirements of the particular
endpoint, a 403 Forbidden with an explanation on which permissions were
missing in order to perform the operation. Certain endpoints will always
succeed, but the results will be filtered based on what the user is
permitted to see. In such case the endpoint will document how to
determine what a user can and cannot see.

Group endpoint
^^^^^^^^^^^^^^

GET /security/group/<path>
''''''''''''''''''''''''''

Retrieves information about this group. The result of the query will
depend on your permissions according to the following rules:

-  If you have READ content group permission on this group, then your
   view is unrestricted. (all fields are present)
-  If you have READ structural group permission on this group, then you
   can know of the existence of this group and all of its sub-groups.
   (``subGroups`` field is present in response)
-  If you have READ content group permission on one of this group's
   sub-groups, then you can see that subgroup as well as any of its own
   subgroups. You can see all members of that group and sub-groups.
   (``allMembers`` and ``subGroups`` fields are present in response)
-  If you have READ structural group permission on one of this group's
   sub-groups, then you can see that subgroup as well as any of its own
   sub-groups. You cannot see any of the members of those groups
   however. (``subGroups`` field is present in response)
-  If you are a member of this group, you can know of the existence of
   this group, but not any of the sub-groups or members. (response is
   empty)
-  If you are a member of a sub-group of this group, you can see that
   sub-group, but nothing else. (``subGroups`` field is present)

These rules are cumulative, so if more than one rule applies, you will
see the combined result. If none of the rules apply, the query will
result in a 403 Forbidden. If certain fields do not apply to your view
of this group, they will be omitted in order to clearly convey that they
are not necessarily empty, you just don't have permission to see
anything related to that field.

-  ``<path>`` is the path of the group in the group hierarchy.

N.B. All users are members of the root group ("/") regardless of whether
they are a member of any other group. Permissions associated with the
root group represent the capabilities of any agent in the system.

Response:

The response body will vary depending on the rules outlined above. In
addition, if you have content or structural READ group permission on
this group and the group does no exist, the response will be a
``404 Not Found``.

.. code:: json

    {
      "members": ["<user_email>", "..."],
      "allMembers": ["<user_email>", "..."],
      "subGroups": ["<group_path>", "..."],
    }

-  ``members`` All users explicitly a member of this group
-  ``allMembers`` All users explicitly and implicitly a member of this
   group. Implicit members of a group refer to the users that are
   explicit members of any of the sub-groups of this group
-  ``subGroups`` Includes the relative paths of all the sub-groups of
   this group. Paths are relative to this group.

Example:

Given the following groups exist in the system:

/corporate -> "Alice" /corporate/engineering -> "Bob"
/corporate/engineering/software -> /corporate/engineering/software/scala
-> "Marcy" /corporate/engineering/hardware -> ("Tom", "Beth")

``GET /security/group/corporate/engineering`` will return

.. code:: json

    {
      "members": ["bob@example.com"],
      "allMembers": ["bob@example.com", "marcy@example.com", "tom@example.com", "beth@example.com"],
      "subGroups": ["software", "software/scala", "hardware"]
    }

POST /security/group/<path>
'''''''''''''''''''''''''''

Creates a new empty group. If any of the parent groups do not exist yet,
they will be created.

*Requires ADD or MODIFY structural group permission.*

Response:

If you have adequate permissions and the group already exists, will
return a ``400 Bad Request``.

PATCH /security/group/<path>
''''''''''''''''''''''''''''

Add or remove users of a group.

*Requires ADD content group permission to add users.* *Requires DELETE
content group permission to remove users.* *Alternatively, the MODIFY
content group permission is sufficient to add and remove users.*

Request:

.. code:: json

    {
      "addUsers": ["<user_email>"],
      "removeUsers": ["<user_email>"]
    }

Response:

If you have adequate permissions, but the group does not exist, the
response will be a ``404 Not Found``

DELETE /security/group/<path>
'''''''''''''''''''''''''''''

Delete this group and all of its sub-groups. All permissions associated
with this group and subgroups as well as shared by this group and
subgroups will immediately become invalid.

*Requires DELETE or MODIFY structural group permission.*

Response:

If you have adequate permissions, but the group does not exist, the
response will be a ``404 Not Found``

Authority endpoint
^^^^^^^^^^^^^^^^^^

GET /security/authority
'''''''''''''''''''''''

Returns all permissions granted to you.

Response:

.. code:: json

    [<permission>]

Permission endpoint
^^^^^^^^^^^^^^^^^^^

GET /security/permission[?transitive]
'''''''''''''''''''''''''''''''''''''

Returns all permissions granted by you. If the ``transitive`` query
param is supplied, will also return all permissions which were derived
from your own.

We plan on adding query parameters in order to filter the result set.

Response:

.. code:: json

    [<permission>]

GET /security/permission/<permission\_id>
'''''''''''''''''''''''''''''''''''''''''

Retrieve a permission by its unique identifier. You may only retrieve
information about permissions shared with you or by you.

If the permission does not exist or you do not have adequate permission
to see it, the response will be a ``404 Not Found``.

Response:

.. code:: json

    <permission>

GET /security/permission/<permission\_id>/children[?transitive]
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

Retrieve all permissions that were directly derived from this
permission. If the ``transitive`` query param is supplied, will also
include permissions which were indirectly derived. You may only retrieve
information about permissions shared with you or by you.

If the permission does not exist or you do not have adequate permission
to see it, the response will be a ``404 Not Found``.

Response:

.. code:: json

    [<permission>]

POST /security/permission
'''''''''''''''''''''''''

Grant new permissions to a given set of users and groups.

Request:

.. code:: json

    {
      "users" : ["<user_email>", "..."],
      "groups" : ["<group_path>", "..."],
      "actions": []
    }

-  ``users`` is a list of user emails, representing the users to whom
   you wish to grant permissions. Users do not need to exist in the
   system at the time the permission is granted. When a user first logs
   into the system, they will be able to perform any action associated
   with permissions granted to their email.
-  ``groups`` is a list of groups with whom you wish to grant
   permissions. Groups DO need to exist in the system prior to granting.
   Providing a group path that points to a group that does not yet exist
   in the system will result in a ``400 Bad Request`` and no new
   permissions will have been granted to users or groups.
-  ``actions`` The actions that the new permissions will allow the
   subjects to perform. All actions must be the same or a subset of
   actions found in your permissions. If that is not the case a
   ``400 Bad Request`` with an appropriate message will be returned and
   no new permissions will have been granted to users or groups.

Although all fields accept arrays, a permission is only ever granted to
ONE subject to perform ONE action. Thus, many permissions will be
created and returned by this endpoint.

Response:

.. code:: json

    [<permission>]

DELETE /security/permission/
''''''''''''''''''''''''''''

Revoke a permission. In order to revoke a permission, you must have a
permission which is a source of authority for the permission you wish to
revoke.

Refer to top-level api description for explanation on the process of
revoking.

Note: Revoking a permission does not guarantee that the subject
associated with that permission no longer has the capability to perform
that action as another subject in the system may have also granted a
permission with the capability to perform the same action. Unless you
are the root user, it is not possible for you to know for sure whether
or not the subject still has the ability to perform the action. As the
root user, all permissions were granted based on your authority so you
can use the ``/security/permission/?transitive`` in order to find out
for sure whether the subject still has permission to perform the action.

If the permission does not exist or you do not have adequate permission
to see it, the response will be a ``404 Not Found``. If you attempt to
revoke one of your own permissions, the response will be a
``400 Bad Request``.

Token endpoint
^^^^^^^^^^^^^^

Here is the json representation of a token:

.. code:: json

    {
      "id": "<token_id>",
      "secret": "<token_hash>",
      "name": "<name>",
      "grantedBy": ["<token_id>", "<user_email>", "<group_path>", "..."],
      "actions": [{
        "operation": "ADD|READ|MODIFY|DELETE",
        "resource": "<filesystem_path>|<group_path>",
        "resourceType": "Structural|Content",
      }]
    }

-  ``secret`` is a cryptographically secure string whose possession
   allows one to perform the action associated with the token.
-  ``name`` an optional field that may or may not have been provided
   upon creation of the token

GET /security/token
'''''''''''''''''''

List tokens that you have created. Does not list tokens that were
created by others based on your authority.

The json representation of the tokens does not contain the ``secret``
field for this endpoint in order to reduce the chance of the secret
leaking. The secret can be retrieved by using the ``id`` endpoint.

Response:

.. code:: json

    [<token>]

GET /security/token/<id>
''''''''''''''''''''''''

Retrieve token for a given id.

You may only retrieve information about a token that you created. If the
token does not exist or was not created by you, the response will be a
``404 Not Found``.

Response:

.. code:: json

    <token>

POST /security/token
''''''''''''''''''''

Create a new token granting the capability to perform the given actions.
All actions must be a subset of your own capabilities. If the later
condition is not satisfied, a ``400 Bad Request`` will be returned.

Request:

.. code:: json

    {
      "name": "",
      "actions": []
    }

-  ``name`` is an optional field

Response:

.. code:: json

    <token>

DELETE /security/token/<id>
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Delete a token. In order to delete a token, you must have a permission
which is a source of authority of the token. If the token does not exist
or was not created by you, a ``404 Not Found`` will be returned.

GET /security/oidc/providers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This endpoint allows clients to obtain the list of configured OpenID
Providers (OPs). Responses will be a JSON array of configurations
similar to the following:

.. code:: json

    [
      {
        "display_name": "Google",
        "client_id": "sdf987wetlkdflkj",
        "openid_configuration": {
          "issuer": "https://accounts.google.com",
          "authorization_endpoint": "https://accounts.google.com/o/oauth2/v2/auth",
          "token_endpoint": "https://www.googleapis.com/oauth2/v4/token",
          "userinfo_endpoint": "https://www.googleapis.com/oauth2/v3/userinfo",
          "jwks": [
            {
              "kty": "RSA",
              "alg": "RS256",
              "use": "sig",
              "kid": "1195d68e49a5571c8e0ab879e70f2cbbc84d6abd",
              "n": "qy5D00Tufwy-EYcnxXmvIJvHodRt4kcv4hWLB6oO1JvzD7vV_IiU_Gp8UvNlr_GsiXGP7ttdV4ctW9hmix2MTUaJ_a881-v9EC0mz6AaMYBq3iIYKm9lIx10omolQ_6RVV6rIfQhnlzCnwmA9UbtynivIyIOHMj9haObtSRpsv1Gq2QUlnSqRozV9KbAXzkdtxgBXB7wcIZYMLxc6YidOACGNNxBgZRdoQnjT4-Gm4mWSrFQylk3tT8CAoNnurJABgUk7xom-pmSlQ4xzROb9hbJ0E4vXKFYJY-xNUt2ehyoJl50lQHFdOgIf-Poy250ZtJRJY02Qt0UKzJ2OquiPw",
              "e": "AQAB"
            },
            {
              "kty": "RSA",
              "alg": "RS256",
              "use": "sig",
              "kid": "b0a61540974b73d939bf80b8846f309ba8575712",
              "n": "rvhjUe0priBSOPUIyJlOvhy0A4OBHvcSYKiTZoc6N2b15eX_QJ5IOEc0844hnAVV2QIxy2UgAhbWLoKlcBI4WVIIqVKgfuv5_LMiE9y_jalrXXag0K63Yr9LuBhllQZbU5Hlkb58rvZN03UJ37iSJQAel20jFx-z8yahXVgmnxZt9iflk0dvLhM7A9p5vhURHaOPhTdUUhsRsBEK97NneX65KDs48Jbl0PhbxYZP_hBexPPiXmPNJ7fC_V2-XPOf_p9goYazjE_fw303ptdk10vL7yP9Mus9ZyaQdZR-3PUGfg78iD6P-LQ_CceuPrE9MJUS1voVn2IRNM8S8iJ36w",
              "e": "AQAB"
            }
          ]
        }
      },
      {
        "display_name": "Our Company OP",
        "client_id": "123455976",
        "openid_configuration": {
          "issuer": "https://op.ourcompany.com",
          "authorization_endpoint": "https://op.ourcompany.com/authorize",
          "token_endpoint": "https://op.ourcompany.com/token",
          "userinfo_endpoint": "https://op.ourcompany.com/userinfo",
          "jwks": [
            {
              "kty": "RSA",
              "kid": "1234",
              "alg": "RS256",
              "use": "sig",
              "n": "2354098udw...2957835lkj"
            },
            {
              "kty": "RSA",
              "kid": "5678",
              "alg": "RS256",
              "use": "sig",
              "n": "skljhdfiugy...39587dlkjsd"
            }
          ]
        }
      }
    ]

Information for certain providers may have a limited lifetime and
subject to expiration. For example, Google is known to rotate its
signing keys about once a day. Eventually, any expiration will be
communicated via standard mechanisms (``Expires`` header and/or
``Cache-Control``). In the meantime, however, if an attempt to validate
an ID Token using a key obtained from this endpoint fails unexpectedly
or one of the provider endpoints ceases to exist, it probably makes
sense to refresh the provider list to ensure one has the most current
information.

