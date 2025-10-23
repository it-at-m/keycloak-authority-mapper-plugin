[open-issues]: https://github.com/it-at-m/refarch/issues
[new-issue]: https://github.com/it-at-m/keycloak-plugins/issues/new/choose
[license]: ./LICENSE
[new-issue-shield]: https://img.shields.io/badge/new%20issue-blue?style=for-the-badge
[made-with-love-shield]: https://img.shields.io/badge/made%20with%20%E2%9D%A4%20by-it%40M-yellow?style=for-the-badge
[license-shield]: https://img.shields.io/github/license/it-at-m/refarch?style=for-the-badge
[itm-opensource]: https://opensource.muenchen.de/

# keycloak-authority-mapper-plugin

[![New issue][new-issue-shield]][new-issue]
[![Made with love by it@M][made-with-love-shield]][itm-opensource]
[![GitHub license][license-shield]][license]

Plugin for mapping Keycloak permissions into user info authorities claim, also called permission mapper.

## Built With

- OpenJDK 21
- Keycloak 26

The Permission Mapper allows users to add permissions related to the user to the Userinfo endpoint.
Since it may be necessary for applications to check access to a resource at the permissions level, this mapper provides
that functionality. Thus, in the Spring Security context, in addition to access checks at the role level
(e.g., `@PreAuthorize("hasRole(...)")`), it is also possible to perform checks at the permissions level
(e.g., `@PreAuthorize("hasAuthority(...)")`). If the permissions of the current user are needed for another client,
the ClientId can be specified as a query parameter. The keyword used for this is `audience`.

## API

### `GET /auth/realms/permission/protocol/openid-connect/userinfo?audience=<clientId>`

Returns the permissions, roles, and user information (claims) of the current user for the specified client.

### `GET /auth/realms/permission/protocol/openid-connect/userinfo`

Returns the permissions, roles, and user information (claims) of the current user. The JSON array `authorities` contains
the user's permissions, while the JSON array `user_roles` includes the application-specific roles of the user.

**Example Response:**

```json
{
  "sub": "b7925f6b-5fae-4a1b-ad3f-9f21f3a4f228",
  "email_verified": false,
  "user_name": "permission.test",
  "preferred_username": "permission.test",
  "user_roles": [
    "ROLE_clientrole_admin"
  ],
  "authorities": [
    "theclient_GUI_Overview",
    "sfmodel_WRITE_Manufacturers",
    "theclient_GUI_OwnApprovals",
    "sfmodel_WRITE_KeyPersons",
    "sfmodel_READ_ReleaseProcess",
    "sfmodel_READ_Applications",
    "theclient_GUI_NewApproval",
    "theclient_GUI_Approvals",
    "sfmodel_READ_Manufacturers",
    "sfmodel_DELETE_Manufacturers",
    "sfmodel_WRITE_Applications",
    "theclient_GUI_ShowApproval",
    "sfmodel_WRITE_ReleaseProcess",
    "theclient_GUI_AdminArea",
    "sfmodel_DELETE_Applications",
    "sfmodel_DELETE_KeyPersons",
    "Default Resource",
    "sfmodel_DELETE_ReleaseProcess",
    "sfmodel_READ_KeyPersons"
  ]
}
```

### Example Workflow using CURL

#### Retrieve Access Token

The Access Token is required to fetch the corresponding permissions, roles, and user information from Keycloak.

The following CURL command retrieves the Access Token from the Keycloak server to perform the subsequent request to the Userinfo endpoint.

```bash
curl -i -v -X POST -d 'client_id=<client-id>' -d 'username=test.admin' -d 'password=Test1234' -d 'grant_type=password' -d 'client_secret=<client_secret>' 'https://<base-url>/realms/<realm>/protocol/openid-connect/token'
````

From the response of the above CURL command, the Access Token from the JSON body should be copied using the key `access_token` and assigned to the variable `TOKEN`.

```bash
TOKEN=eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSl...1lIjoid
```

#### Call the Userinfo Endpoint

With the following command, the endpoint can then be called.

##### When using the global Permission Provider

##### When using a locally running test instance of the Permission Provider

```bash
curl -i -v -X GET -H "Authorization: Bearer $TOKEN" "https://<base-url>/auth/realms/<realm>/protocol/openid-connect/userinfo"
```

## Configuration
### In Keycloak

To use the Permission Mapper, a new mapper of type User's authorities must simply be added under Mappers in the corresponding client.

## Roadmap

See the [open issues][open-issues] for a full list of proposed features (and known issues).

## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any
contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please open an issue with the tag "enhancement", fork the repo and
create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Open an issue with the tag "enhancement"
2. Fork the Project
3. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
4. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
5. Push to the Branch (`git push origin feature/AmazingFeature`)
6. Open a Pull Request

More about this in the [CODE_OF_CONDUCT](./.github/CODE_OF_CONDUCT.md) file.

## License

Distributed under the MIT License. See [LICENSE](LICENSE) file for more information.

## Contact

it@M - <opensource@muenchen.de>
