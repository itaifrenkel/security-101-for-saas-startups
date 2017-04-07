Startups find it hard to engage multiple security vendors. This vendor narrowing tool should help you with preparing a shortlist by entering your specific startup security requirements checklist, and getting a list of vendors and product names. We also provide sample requirements to get you started.

## Disclaimers ##
* This tool should be used for narrowing down a selection of security vendors (vendor shortlist) and should not be solely used for making a purchase decision. The information is provided as-is and we cannot take responsibility for your security tools choices.  You would still need to contact the vendor, verify claims, read reviews and ask for a POC, before making a purchasing decision. 
* The information cataloged here is incomplete, inaccurate and out-of-date. If you find such an issue please open a Pull Request that includes the requested change and a link to a screencast showing that feature or technical documentation of the product. All vendors use massive marketing mambo-jambo, however they would not fake a screencast or their own official technical documentation. 
* The information cataloged here is licensed under a Creative Commons Attribution 4.0 International License. Feel free to fork this repo and change it.


## This tool is highly opinionated ##
* Security systems, just like your systems, requires integration, continous monitoring and upgrades. If you choose a tool that is too complex for your startup, you would be wasting time and money. This is why you should first read the "Security 101 for SaaS startups guide". 
* This is also why this catalog focuses solely on SaaS (hosted) solutions, since they were built from the ground up with usability in mind. The flip side of relying on a hosted security solution is that if such a SaaS is compromized, many companies including yours might suffer collateral damage. Therefore, we focus on vendors that have soc-2 certification or are in the process of obtaining such certification.
* Even if you do find a security tool that is perfect for your organization, it would still be used by imperfect humans. Therfore we focus on tools that have identity baked into them. The minimum is users,roles/groups and devices. Hopefully it also provides role based policies (e.g. finance and management must have X feautre enabled on their laptop and mobile phones). At the end of the day, for most alerts you would contact the employee and walk them through whatever they did wrong (e.g. please don't browse website X using this laptop, ok?).
* The most important security tool is upgrading to the latest version of Windows or macOS, and installing the latest security updates. If you're not doing even that, stop reading now, fix this, and come back when you're done. 
* Ubuntu Desktop is a great operating system too, but it's not covered here. The reason is that most security vendors make a living by selling to enterprise customers, and they rarly use Ubuntu Desktop, so the vendor selection is very limited.

## Prerequisites ##
```
git clone forter/security-101-for-saas-startups
```

## Usage ##
We provide a sample of security requirements yaml files. These files are hierarchal.

solution category:  
```iam``` stands for identity and access management  
```epp``` enterprise protection platform  
```emm``` enterprise mobility management  

AND conditions followed by a list of requirements:  
`AND_MUST` - requirements that are mandatory for a service to be included in the results  
`AND_WANT` - requirements that are expected, and moves the service to the top of the results  
`AND_NICE` - requirements that are nice to have, and moves the service slightly to the top of the results  
`AND_EXCLUDE` - requirements that would exclude a service from the results  

OR conditions followed by a list of requirements:  
`OR` - requirements that are interchangeable

`./vendors.py filter -r requirements.yaml`

# Example #
```
- iam:
    AND_MUST:
    - iam.features.sso
    - iam.features.mfa.otp
    - iam.features: 
        OR:
        - radius
        - radius_gateway
    AND_WANT:
    - suite.security.soc2_type2
    AND_NICE:
    - iam.features.mfa.yubikey
- epp:
    AND_MUST:
    - epp.tests.windows.avtest.realworld: 3
    AND_WANT:
    - epp.tests.windows.avtest.files : 3
    - suite.security.soc2_type2
    AND_EXCLUDE:
    - suite.name : 'terrible anti-virus'
```
The first requirement is for an Identity and Access Management services that provides Single Sign-On with Multi-Factor Authentication and exposes radius API or provides a radius gateway. Services that achieved soc2 type2 certification, between them suites that support yubikey mfa will appear first.
The second requirement is for Windows Endpoint Protection Platform that passed avtest real world tests. Suites that achieved top avtest file sample scores and achieved soc2 certification will appear on top. Finally it excludes the suite named "terrible anti-virus".
`

## Security Catalog criteria ##
| criteria | description |
|----------|-------------|
| **vendor** | General information about the company producting the security service |
|vendor.name |  The official name of the company |
|vendor.headquarters | The country in which the company is listed. This is important when you want to exclude/include seucurity tools from countries that have specific regulations.|
| **suite** | General information about the suite of tools that ussually have (or will have) a consolidated management console |
| suite.name | The marketing name for the suite |
| suite.landing_page | URL which provides more information about the product suite | vendor website|
| suite.login_page | URL of the SaaS product customer login page|
| suite.docs_page | URL of the SaaS technical documentation |
| **suite.security** | Confidence that the SaaS you are using won't get compromized (avoid collateral damage) |
| suite.security.soc2_type2 | True if the SaaS achieved SOC2 Type2 compliance|
| suite.security.csa_star_level2 | True if the SaaS recieved CSA Star attestation|
| suite.security.iso_27001_2013 | True if the SaaS achieved ISO 27001 2013 certification|
| suite.security.iso_27018_2014 | True if the SaaS achieved ISO 27018 2014 certification| vendor website|
| suite.security.hippa | True if the SaaS achieved HIPPA compliance|
| suite.security.fedramp_ato | True if the SaaS achieved FedRAMP compliance and Auhtority to Operate| vendor website|
| suite.security.pen_tests | True if the SaaS is continously going through penetration tests |
| suite.security.bug_bounty | True if the SaaS has a public bug bounty program |
| **iam** | Identity and Access Management features |
| iam.products | List of marketing name of products that are part of the suite that implement Identity and Access Management |
| iam.features.users | Directory of users |
| iam.features.roles | Role based policies (sometimes called groups) |
| iam.features.devices | Directory of devices linked to the user |
| iam.features.sso | Single Sign-On App Catalog. Check that it includes all of the SaaS products you are using|vendor website|
| iam.features.lifecycle | Extends SSO by automatic provisioning and deprovisioning of users from 3rd party services and syncing with other directories |
| iam.features.saml | Exposes SAML Sign Sign-On API |
| iam.features.mfa_otp | Supports Multi Factor Authentication via Google Authenticator or similar One-Time-Password|
| iam.features.mfa_push | Supports Multi Factor Authentication via a mobile app with push notifications |
| iam.features.mfa_yubikey | Supports Multi Factor Authentication via YubiKey |
| iam.features.browser_plugin | Supports automatic login even to strange websites that require a browser plugin |
| iam.features.radius_gateway | Provides a self-hosted radius gateway that exposes the radius protocol |
| iam.features.radius | Exposes radius protocol |
| iam.features.ldap_sync | Provides a self-hosted synchornization tool between the hosted directory and ldap (active directory, AWS Simple AD, etc...) |
| **epp** | Endpoint Protection Platform (antivirus, personal firewall, device control, ...). |
| **epp.tests** | Independent anti-malware tests |
| epp.tests.windows.avtest.realworld | Based on latest Business Windows Client Windows 10 excel file, 3 means real-world 100% detection rate, at most 1 false positive, 2 means real-world above 99% detection rate at most 1 false positive, 1 means lesser results,0 not tested | https://www.av-test.org/en/press/test-results/ |
| epp.tests.windows.avcomparatives.realworld | Based on latest Real World Protection Test, 3 means 100% detection and less than 10% false positives, 2 means 99% detection and less than 10% false positives, 1 means lesser results, 0 means not tested | https://chart.av-comparatives.org/chart1.php |
| epp.tests.windows.avtest.files | Based on latest Business Windows Client Windows 10 excel file, 3 means samples 100% detection rate, at most 1 false positive during system scan, 2 means samples above 99% detection rate at most 1 false positive during system scan, 1 means lesser results,0 not tested | https://www.av-test.org/en/press/test-results/ |
| epp.tests.windows.avcomparatives.files | Based on latest File Detection Test, 3 means 100% detection, 2 means above 99% detection, 1 means lesser results, 0 means not tested | https://chart.av-comparatives.org/chart1.php |
| epp.tests.mac.avtest.files | Based on latest Home User MacOS excel file, 3 means 100% mac malware detection and 0 false positives, 2 means at most one mac malware detection miss and at most one false positives, 1 means lesser results, 0 not tested | https://www.av-test.org/en/press/test-results/ |
| epp.tests.mac.avcomparatives.files| Based on latest MacOS PDF file, 3 means 100% mac malware detection, 2 means above 95% detection, 1 means lesser results, 0 not tested |  https://www.av-comparatives.org/mac-security-reviews/ |
|**epp.features.mac.prevent** | Prevents malware from ever running on the machine |
|epp.features.mac.prevent.firewall | Block network protocols, such as denying all incoming tcp connections | vendor docs |
|epp.features.mac.prevent.hips | Prevent remote malware from attacking legitimate processes through the network (port scanning, buffer overrun, DoS, etc..) | vendor docs |
|epp.features.mac.prevent.malicousurl | url blocking and malicous javascript detection | 
|epp.features.mac.prevent.unauthorziedapps | white/black listing of applications | 
|epp.features.mac.prevent.removeablemedia | Block bluetooth/usb/cd/etc...| vendor docs |
|epp.features.mac.prevent.executablefiles | Prevent malware from running | vendor docs |
|**epp.features.mac.detect** | Detect and react to malicous behavior of malware (that was not caught by the prevention modules)| 
|epp.features.mac.detect.exploit | Detect attempt to exploit legitimate process vulnerability, such as in adobe/word | 
|epp.features.mac.detect.ransomeware | Detect ransomeware behavior and rollback file encryption | vendor docs
| epp.virustotal.malware | anti-virus products that participate with virustotal enjoy a stream of the latest malware files| https://www.virustotal.com/en/about/credits/ |
| epp.virustotal.website | web filtering products that participate with virustotal enjoy a stream of the latest rouge web urls | https://www.virustotal.com/en/about/credits/ |
