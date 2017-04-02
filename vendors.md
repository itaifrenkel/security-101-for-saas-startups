Startups also can't efford engaging with multiple security vendors. This vendor narrowing tool should help you with preparing a shortlist by entering your specific startup requirements, and getting a list of vendors and product names. We also provide sample requirements to get you started.

## Disclaimers ##
* This tool should be used for narrowing down a selection of security vendors (vendor shortlist) and should not be solely used for making a purchase decision. The information is provided as-is and we cannot take responsibility for your security tools choices.  You would still need to contact the vendor, verify claims, read reviews and ask for a POC, before making a purchasing decision. 
* The information cataloged here is incomplete, inaccurate and out-of-date. If you find such an issue please open a Pull Request that includes the requested change and a link to a screencast showing that feature or technical documentation of the product. All vendors use massive marketing mambo-jambo, however they would not fake a screencast or their own official technical documentation. 
* The information cataloged here is licensed under a Creative Commons Attribution 4.0 International License. Feel free to fork this repo and change it.


## This tool is highly opinionated ##
* Security systems, just like your systems, requires integration, continous monitoring and upgrades. If you choose a tool that is too complex for your startup, you would be wasting time and money. This is why you should first read the "Security 101 for SaaS startups guide". 
* This is also why this catalog focuses solely on SaaS (hosted) solutions, since they were built from the ground up with usability in mind. The flip side of relying on a hosted security solution is that if such a SaaS is compromized, many companies including yours might suffer collateral damage. Therefore, we focus on vendors that have soc-2 certification or are in the process of obtaining such certification.
* Even if you do find a security tool that is perfect for your organization, it would still be used by imperfect humans. Therfore we focus on tools that have identity baked into them. The minimum is users,roles/groups and devices. Hopefully it also provides role based policies (e.g. finance and management must have X feautre enabled on their laptop and mobile phones). At the end of the day, for most alerts you would contact the employee and walk them through whatever they did wrong (e.g. please don't browse website X using this laptop, ok?).
* The most important security tool is upgrading to the latest version of Windows or MacOS, and installing the latest security updates. If you're not doing even that, stop reading now, fix this, and come back when you're done. 
* Ubuntu Desktop is a great operating system too, but it's not covered here. The reason is that most security vendors make a living by selling to enterprise customers, and they rarly use Ubuntu Desktop, so the vendor selection is very limited.

## Prerequisites ##
```
git clone forter/security-101
docker run --name security-101 forter/security-101
```

## Basic Usage ##
We provide a sample of security requirements yaml files that include "must", "want", "nice", "exclude" requirement levels. 

The following requirement is for an Identity and Access Management services that provides Single Sign-On with Multi-Factor Authentication and exposes radius API or provides a radius gateway. Results are sorted by score, and Suites that also support yubikey, and suites that achieved CSA Star attestation will have higher score and will appear higher. The requirement also states the need for an Endpoint Protection Platform for windows that has maximum usability score (no pop-ups, false positives), very good protection score. It gives higher score for suites that ahieved maximum performance and protection score, and are soc2_type2 compliant. It excludes the suite named 'terrible anti-virus'.

```
iam:
  must:
  - suite.security.soc2_type2
  - suite.iam.features.sso
  - suite.iam.features.mfa.otp
  - suite.iam.features: [radius, radius_gateway]
  want:
  - suite.iam.features.mfa.yubikey
  nice:
  - suite.security.csa_star_level2

epp:
- must:
  - suite.epp.features.windows.avtest.protection: 5
  - suite.epp.features.windows.avtest.usability: 6
- want:
  - suite.security.soc2_type2
  - suite.epp.features.windows.avtest.protection: 6
  - suite.epp.features.windows.avtest.performance: 6
- exclude:
  - suite.name : 'terrible anti-virus'
```

`docker exec -d security-101 vendors filter < requirements.yaml`

## Advanced Usage ##
You can use elasticsearch query syntax for more advanced queries
```
docker exec -d security-101 vendors querygen < requirements.yaml > query.json
<manually edit query.json>
docker exec -d security-101 vendors query < query.json
```

## Security Catalog criteria ##
| criteria | description | evidence |
|----------|-------------|----------|
| *vendor* | General information about the company producting the security service ||
|vendor.name |  The official name of the company | vendor website |
|vendor.headquarters | The country in which the company is listed. This is important when you want to exclude/include seucurity tools from countries that have specific regulations.| vendor website |
| *suite* | General information about the suite of tools that ussually have (or will have) a consolidated management console ||
| suite.name | The marketing name for the suite ||
| suite.landing_page | URL which provides more information about the product suite | vendor website|
| suite.login_page | URL of the SaaS product suite | vendor website |
| *suite.security* | Confidence that the SaaS you are using won't get compromized (avoid collateral damage) |
| suite.security.soc2_type2 | True if the SaaS achieved SOC2 Type2 compliance| vendor website |
| suite.security.csa_star_level2 | True if the SaaS recieved CSA Star attestation| vendor website |
| suite.security.iso_27001_2013 | True if the SaaS achieved ISO 27001 2013 certification| vendor website |
| suite.security.iso_27018_2014 | True if the SaaS achieved ISO 27018 2014 certification| vendor website|
| suite.security.hippa | True if the SaaS achieved HIPPA compliance| vendor website |
| suite.security.fedramp_ato | True if the SaaS achieved FedRAMP compliance and Auhtority to Operate| vendor website|
| suite.security.pen_tests | True if the SaaS is continously going through penetration tests | vendor website |
| suite.security.bug_bounty | True if the SaaS has a public bug bounty program | vendor website |
| *suite.iam* | Identity and Access Management features ||
| suite.iam.products | List of marketing name of products that are part of the suite that implement Identity and Access Management |
| suite.iam.features.users | Directory of users |vendor docs|
| suite.iam.features.roles | Role based policies (sometimes called groups) |vendor docs|
| suite.iam.features.devices | Directory of devices linked to the user |vendor docs|
| suite.iam.features.sso | Single Sign-On App Catalog. Check that it includes all of the SaaS products you are using|vendor website|
| suite.iam.features.lifecycle | Extends SSO by automatic provisioning and deprovisioning of users from 3rd party services and syncing with other directories |vendor docs|
| suite.iam.features.saml | Exposes SAML Sign Sign-On API |vendor docs|
| suite.iam.features.mfa_otp | Supports Multi Factor Authentication via Google Authenticator or similar One-Time-Password|vendor docs|
| suite.iam.features.mfa_push | Supports Multi Factor Authentication via a mobile app with push notifications |vendor docs|
| suite.iam.features.mfa_yubikey | Supports Multi Factor Authentication via YubiKey |vendor docs|
| suite.iam.features.browser_plugin | Supports automatic login even to strange websites that require a browser plugin |vendor docs|
| suite.iam.features.radius_gateway | Provides a self-hosted radius gateway that exposes the radius protocol ||
| suite.iam.features.radius | Exposes radius protocol ||
| suite.iam.features.ldap_sync | Provides a self-hosted synchornization tool between the hosted directory and ldap (active directory, AWS Simple AD, etc...) ||
| suite.epp | Endpoint Protection Platform (antivirus, personal firewall, device control, ...) |
| suite.epp.features.windows.avtest.performance | performance impact score between 0 to 6 | https:////www.av-test.org/en/antivirus/business-windows-client/windows-10/ |
| suite.epp.features.windows.avtest.protection | protection score between 0 to 6 | https:////www.av-test.org/en/antivirus/business-windows-client/windows-10/ |
| suite.epp.features.windows.avtest.usability | usability score between 0 to 6 | https:////www.av-test.org/en/antivirus/business-windows-client/windows-10/ |

virustotal_malware - endpoint protection vendors registered with virustotal enjoy a stream of the latest malware.
virustotal_unsafe_websites - endpoint protection vendors registered with virustotal enjoy a stream of the latest urls
