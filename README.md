<!-- SHIELDS -->
[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![License][license-shield]][license-url]

<p>
  <a href="https://github.com/helsingborg-stad/wordpress-plugin-unzipper">
    <img src="docs/images/hbg-github-logo-combo.png" alt="Logo" width="300">
  </a>
</p>
<h3>Wordpress plugin unzipper</h3>
<p>
  <br />
  <a href="https://github.com/helsingborg-stad/wordpress-plugin-unzipper/issues">Report Bug</a>
  ·
  <a href="https://github.com/helsingborg-stad/wordpress-plugin-unzipper/issues">Request Feature</a>
</p>

## Summary
This action will unzip zip files containing plugins and rsync them all into a wordpress plugin folder.

## Parameters

| Parameter name              | Description                                                                  | Required |
|-----------------------------|------------------------------------------------------------------------------|----------|
| deploy-host                 | Host domain or ip                                                            | true     |
| deploy-host-path            | Host deployment absolute path to wordpress root folder                       | true     |
| deploy-port            | Host deploy port                                                             | true     |
| deploy-host-user            | Host deploy ssh user name                                                    | true     |
| deploy-host-user-key        | Host deploy ssh user key                                                     | true     |
| deploy-host-web-server-user | Host web server user                                                         | true     |
| github-token                | Github token for github npm package usage, use built in secrets.GITHUB_TOKEN | true     |
| plugin-repo                 | A repository with zip-files to unzip in a wordpress folder.                  | true     |



## Secrets
All parameters are sensitive information and should ONLY use [Github secrets.](https://docs.github.com/en/actions/security-guides/encrypted-secrets).  
Secrets can be added on organisation level spanning all repositories in the org or on a repository level where Github actions in the current repository will be the only one able to read it.

## Usage
In your site repository:
- Create a file in `.github/workflows/` named ex. `deploy-branchname.yml`.
- Use the following template.
- Any build/deploy step should prefferably be executed before this action to avoid syncing issues when for example rsync is used with --delete flag.
```yaml
name: Unzip plugins

on:
  push:
    branches: [ branchname ]
    paths-ignore:
      - .github/**

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

  unzip:
    needs: build
    runs-on: ubuntu-latest

    steps:
    - uses: helsingborg-stad/wordpress-plugin-unzipper@master
      with:
        deploy-host: ${{ secrets.DEPLOY_REMOTE_HOST_DOMAIN_SE }}
        deploy-port: ${{ secrets.DEPLOY_REMOTE_PORT_DOMAIN_SE }}
        deploy-host-path: ${{ secrets.DEPLOY_REMOTE_PATH_DOMAIN_SE }}
        deploy-host-user: ${{ secrets.DEPLOY_REMOTE_USER }}
        deploy-host-user-key: ${{ secrets.DEPLOY_KEY }}
        deploy-host-web-server-user: ${{ secrets.WEB_SERVER_USER }}
        github-token: ${{ secrets.GITHUB_TOKEN }}
        plugin-repo: ${{ secrets.ZIPPED_PLUGINS_REPO }}
  ```
- Make sure branchname is matching the branch you would want to deploy.
- Replace secret names with macthing secrets.



## Contributing

Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request



## License

Distributed under the [MIT License][license-url].


<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/helsingborg-stad/wordpress-plugin-unzipper.svg?style=flat-square
[contributors-url]: https://github.com/helsingborg-stad/wordpress-plugin-unzipper/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/helsingborg-stad/wordpress-plugin-unzipper.svg?style=flat-square
[forks-url]: https://github.com/helsingborg-stad/wordpress-plugin-unzipper/network/members
[stars-shield]: https://img.shields.io/github/stars/helsingborg-stad/wordpress-plugin-unzipper.svg?style=flat-square
[stars-url]: https://github.com/helsingborg-stad/wordpress-plugin-unzipper/stargazers
[issues-shield]: https://img.shields.io/github/issues/helsingborg-stad/wordpress-plugin-unzipper.svg?style=flat-square
[issues-url]: https://github.com/helsingborg-stad/wordpress-plugin-unzipper/issues
[license-shield]: https://img.shields.io/github/license/helsingborg-stad/wordpress-plugin-unzipper.svg?style=flat-square
[license-url]: https://raw.githubusercontent.com/helsingborg-stad/wordpress-plugin-unzipper/master/LICENSE
