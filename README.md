# Community-Maintained Documentation for the Discord API

[![](https://github.com/apacheli/community-docs/actions/workflows/ci.yaml/badge.svg?branch=master)](https://github.com/apacheli/community-docs/actions/workflows/ci.yaml)
[![](https://discord.com/api/guilds/81384788765712384/widget.png)](https://discord.gg/discord-api)

### About

The source for the _unofficial_ Discord API documentation.

[You can read the compiled documentation here.](https://apacheli.github.io/community-docs/)

### Contributing

- Clone the repository: `$ git clone https://github.com/apacheli/community-docs`
- [Install mdBook](https://rust-lang.github.io/mdBook/guide/installation.html)
- [Install dprint](https://dprint.dev/install/)
- Run the documentation locally: `$ mdbook serve`
- Make the necessary changes. See [the style guide](#style-guide) for more
  information.
- Run the formatter from the root of the directory: `$ dprint fmt`
- Run the checker to make sure that everything is formatted: `$ dprint check`
- Create a pull request.

By contributing to this repository, you agree to the terms of
[the community-docs license](./LICENSE).

### Style Guide

- Each page should flow smoothly from top to bottom.
- Keep the wording simple and consise.
- Do not use contractions (e.g. use "do not" over "don't").

### Useful Links

- [Discord API Server](https://discord.gg/discord-api)
