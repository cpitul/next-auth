# Contributing guide

Contributions and feedback on your experience of using this software are welcome.

This includes bug reports, feature requests, ideas, pull requests, and examples of how you have used this software.

Please see the [Code of Conduct](CODE_OF_CONDUCT.md) and follow any templates configured in GitHub when reporting bugs, requesting enhancements, or contributing code.

Please raise any significant new functionality or breaking change an issue for discussion before raising a Pull Request for it.

## For contributors

Anyone can be a contributor. Either you found a typo, or you have an awesome feature request you could implement, we encourage you to create a Pull Request.

### Pull Requests

- The latest changes are always in `main`, so please make your Pull Request against that branch.
- Pull Requests should be raised for any change
- Pull Requests need approval of a [core contributor](https://next-auth.js.org/contributors#core-team) before merging
- We use ESLint/Prettier for linting/formatting, so please run `pnpm lint:fix` before committing to make resolving conflicts easier (VSCode users, check out [this ESLint extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) and [this Prettier extension](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) to fix lint and formatting issues in development)
- We encourage you to test your changes, and if you have the opportunity, please make those tests part of the Pull Request
- If you add new functionality, please provide the corresponding documentation as well and make it part of the Pull Request

### Setting up local environment


A quick guide on how to setup _next-auth_ locally to work on it and test out any changes:

1. Clone the repo:

```sh
git clone git@github.com:nextauthjs/next-auth.git
cd next-auth
```

2. Set up the correct pnpm version, using [Corepack](https://nodejs.org/api/corepack.html). Run the following in the project'a root:

```sh
corepack enable pnpm
```

(Now, if you run `pnpm --version`, it should print the same verion as the `packageManager` property in the [`package.json` file](https://github.com/nextauthjs/next-auth/blob/main/package.json))

3. Install packages. Developing requires Node.js v18:

```sh
pnpm install
```

4. Populate `.env.local`:

Copy `apps/dev/.env.local.example` to `apps/dev/.env.local`, and add your env variables for each provider you want to test.

```sh
cd apps/dev
cp .env.local.example .env.local
```

> NOTE: You can add any environment variables to .env.local that you would like to use in your dev app.
> You can find the next-auth config under`apps/dev/pages/api/auth/[...nextauth].js`.

5. Start the developer application/server:

```sh
pnpm dev
```

Your developer application will be available on `http://localhost:3000`

That's it! 🎉

If you need an example project to link to, you can use [next-auth-example](https://github.com/iaincollins/next-auth-example).

#### Hot reloading

When running `pnpm dev`, you start a Next.js developer server on `http://localhost:3000`, which includes hot reloading out-of-the-box. Make changes on any of the files in `src` and see the changes immediately.

> NOTE: When working on CSS, you will have to manually refresh the page after changes. The reason for this is our pages using CSS are server-side rendered (using API routes). (Improving this through a PR is very welcome!)

> NOTE: The setup is as follows: The development application lives inside the `app` folder, and whenever you make a change to the `src` folder in the root (where next-auth is), it gets copied into `app` every time (gitignored), so Next.js can pick them up and apply hot reloading. This is to avoid some annoying issues with how symlinks are working with different React builds, and also to provide a super-fast feedback loop while developing core features.

#### Providers

If you think your custom provider might be useful to others, we encourage you to open a PR and add it to the built-in list so others can discover it much more easily! You only need to add two changes:

1. Add your config: [`src/providers/{provider}.js`](https://github.com/nextauthjs/next-auth/tree/main/packages/next-auth/src/providers) (Make sure you use a named default export, like `export default function YourProvider`!)
2. Add provider documentation: [`www/docs/providers/{provider}.md`](https://github.com/nextauthjs/next-auth/tree/main/www/docs/providers)

That's it! 🎉 Others will be able to discover this provider much more easily now!

You can look at the existing built-in providers for inspiration.

#### Databases

If you would like to contribute to an existing database adapter or help create a new one, head over to the [nextauthjs/adapters](https://www.github.com/nextauthjs/adapters) repository and follow the instructions provided there.

#### Testing

Tests can be run with `pnpm test`.

Automated tests are currently crude and limited in functionality, but improvements are in development.

## For maintainers

We use [a custom script](https://github.com/nextauthjs/next-auth/blob/main/scripts/release/index.ts) together with [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0) to automate releases. This makes the maintenance process easier and less error-prone. Please study the "Conventional Commits" site to understand how to write a good commit message.

When accepting Pull Requests, make sure the following:

- Use "Squash and merge"
- Make sure you merge contributor PRs into `main`
- Rewrite the commit message to conform to the `Conventional Commits` style.
  - Using `fix` releases a patch (x.x.1)
  - Using `feat` releases a minor (x.1.x)
  - Using `feat` when `BREAKING CHANGE` is present in the commit message releases a major (1.x.x)
- Optionally link issues the PR will resolve (You can add "close" in front of the issue numbers to close the issues automatically, when the PR is merged. `semantic-release` will also comment back to connected issues and PRs, notifying the users that a feature is added/bug fixed, etc.)

### Skipping a release

If a commit contains `[skip release]` in their message, it will be excluded from the commit analysis and won't participate in the release type determination. This is useful, if the PR being merged should not trigger a new `npm` release.