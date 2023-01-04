# Getting Captain working with Mocha

1. 🧪 Ensure Mocha produces JSON output

By default, Mocha can produce JSON output with `npx mocha "test-pattern" -R json --reporter-option output=report.json`,
but it will squash the default `spec` reporter. To use multiple reporters with Mocha, you need to install a
[library](https://github.com/rwx-research/mocha-multi-reporters). Once installed, create a config file at
`multi-reporters.json` containing:

```json
{
  "reporterEnabled": "spec,json",
  "jsonReporterOptions": {
    "output": "tmp/report.json"
  }
}
```

Then, you can use both the spec and json reporters with this command:

```sh
npx mocha "test-pattern" -R @rwx-research/mocha-multi-reporters --reporter-options configFile=multi-reporters.json
```

2. 🔐 Create an Access Token

Create an Access Token for your organization within [Captain][captain] ([more documentation here][create-access-token]).

Add the new token as an action secret to your repository. Conventionally, we call this secret `RWX_ACCESS_TOKEN`.

3. 💌 Install the Captain CLI, and then call it when running tests

See the [full documentation on test suite integration][test-suite-integration].

```yaml
  - name: Run tests
    run: |
      captain run --suite-id captain-examples-mocha --test-results tmp/report.json -- \
        npm test
    env:
      RWX_ACCESS_TOKEN: ${{ secrets.RWX_ACCESS_TOKEN }}
```

4. 🎉 See your test results in Captain!

Take a look at the [final workflow!][workflow-with-captain]

[captain]: https://account.rwx.com/deep_link/manage/access_tokens
[create-access-token]: https://www.rwx.com/docs/access-tokens
[workflow-with-captain]: https://github.com/captain-examples/mocha/blob/main/.github/workflows/ci.yml
[test-suite-integration]: https://www.rwx.com/captain/docs/test-suite-integration
