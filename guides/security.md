# Security

In general, developers should be mindful of application security risks, and should adhere to 
the [OWASP Top 10](https://www.owasp.org/images/7/72/OWASP_Top_10-2017_%28en%29.pdf.pdf) as best 
possible.

Users should also be aware of [ITSD security policies](https://innerspace.stsci.edu/display/isec).
 
## Tools for security practices

The following tools are recommend for the use with different languages for static analysis
* Python: [bandit](https://pypi.org/project/bandit/)
* Java: [sonarqube](https://www.sonarqube.org/)

## Secure information 

The following items should never be committed in a source code or GitHub issues/pull requests:

- Account credentials of any kind (e.g. database usernames and passwords)
- Internal directory structures or file paths
- Machine names
- Proprietary data

If code needs to be aware of this information, it should be stored in a configuration file that is 
not part of the repository - e.g. using [environment variables](https://12factor.net/config) loaded
into the application at runtime.

## Additional resources and recommendations

* [Web Application Security Tools](https://innerspace.stsci.edu/display/ASB/Web+Application+Security+Tools)
* [ASB Security Assurance Process](https://innerspace.stsci.edu/display/ASB/ASB+Security+Assurance+Process+-+2019+March)
