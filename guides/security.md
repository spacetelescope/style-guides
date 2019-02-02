# Security


The following items should never be committed in a source code or GitHub issues/pull requests:

- Account credentials of any kind (e.g. database usernames and passwords)
- Internal directory structures or filepaths
- Machine names
- Proprietary data

If code needs to be aware of this information, it should be stored in a configuration file that is 
not part of the repository - e.g. using [environment variables](https://12factor.net/config) loaded
into the application at runtime.

Additionally, developers should be mindful of application security risks, and should adhere to 
the [OWASP Top 10](https://www.owasp.org/images/7/72/OWASP_Top_10-2017_%28en%29.pdf.pdf) as best 
possible.

