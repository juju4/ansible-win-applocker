[![Build Status - Master](https://travis-ci.org/juju4/ansible-win-applocker.svg?branch=master)](https://travis-ci.org/juju4/ansible-win-applocker)
[![Build Status - Devel](https://travis-ci.org/juju4/ansible-win-applocker.svg?branch=devel)](https://travis-ci.org/juju4/ansible-win-applocker/branches)(Syntax Only)

[![Appveyor - Master](https://ci.appveyor.com/api/projects/status/fajn5w2f4v52lfyj?svg=true)](https://ci.appveyor.com/project/juju4/ansible-win-applocker)
![Appveyor - Devel](https://ci.appveyor.com/api/projects/status/fajn5w2f4v52lfyj/branch/devel?svg=true)

# Windows AppLocker ansible role

Ansible role to setup AppLocker (application whitelisting) on windows system.

## Requirements & Dependencies

### Ansible
It was tested on the following versions:
 * 2.3 (Not working! [ansible#23940](https://github.com/ansible/ansible/issues/23940) = template upload failing)
 * 2.4
 * 2.5b2

### Operating systems

Tested in Appveyor

## Example Playbook

Just include this role in your list.
For example

```
- host: all
  roles:
    - juju4.win-applocker
```

Run
```
$ ansible -i inventory -m win_ping win --ask-pass
$ ansible-playbook -i inventory --limit win site.yml
```

## Variables

See defaults/main.yml for full scope

## Continuous integration

This role has a travis basic test (for github, syntax check only), Appveyor test and a Vagrantfile (test/vagrant).

```
$ cd /path/to/roles/juju4.win-applocker/test/vagrant
$ vagrant up
$ vagrant provision
$ vagrant destroy
$ ansible -i .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory -m win_ping -e ansible_winrm_server_cert_validation=ignore -e ansible_ssh_port=55986 all
```

## Troubleshooting & Known issues

* Trying to track [LOLBAS](https://github.com/api0cradle/LOLBAS) but remember to test in your environment!

## FAQ


## References

* [Enable AppLocker in audit mode, Win Enterprise+](https://technet.microsoft.com/en-us/library/dd759131(v=ws.11).aspx)
* [AppLocker Design Guide, Microsoft](https://docs.microsoft.com/en-us/windows/device-security/applocker/applocker-policies-design-guide)
* [Microsoft recommended block rules](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/microsoft-recommended-block-rules)
* [Microsoft recommended driver block rules](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/microsoft-recommended-driver-block-rules)

* [A WDAC audit-mode policy that will log all non-Windows-signed driver loads and any driver that is not WHQL or EV signed. Feb 2021](https://gist.github.com/mgraeber-rc/6435330f0e8d659636b071b39d34660b)
```
$allowMicrosoftPolicy = "c:\windows\schema\CodeIntegrity\ExamplePolicies\AllowMicrosoft.xml"
$denyAllAuditPolicy = "c:\windows\schema\CodeIntegrity\ExamplePolicies\DenyAllAudit.xml"
Merge-CIPolicy -OutputFilePath CurrentPolicy.xml -PolicyPaths $allowMicrosoftPolicy,$denyAllAuditPolicy
$outputPath = "C:\Windows\System32\CodeIntegrity\SIPolicy.p7b"
ConvertFrom-CIPolicy -XmlFilePath CurrentPolicy.xml -BinaryFilePath $outputPath
Invoke-CimMethod -Namespace root/microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = $outputPath }
```
* [Application Control is one of the most common recommendations for making your environment that of an Apex Defender. While getting started can be daunting, these three simple lines of PowerShell are a HUGE start, and will audit every binary not from Windows or Microsoft. Dec 2020](https://twitter.com/Lee_Holmes/status/1340301433581654016)
* [Threat Detection using Windows Defender Application Control (Device Guard) in Audit Mode, Jun 2018](https://posts.specterops.io/threat-detection-using-windows-defender-application-control-device-guard-in-audit-mode-602b48cd1c11?gi=58f5d8b7b8b5)
* [Steps to Deploy Windows Defender Application Control, Apr 2018 - with recommended applications to block](https://docs.microsoft.com/en-us/windows/security/threat-protection/device-guard/steps-to-deploy-windows-defender-application-control)
* [Adventures in Extremely Strict Device Guard Policy Configuration Part 1 — Device Drivers, Nov 2017](https://posts.specterops.io/adventures-in-extremely-strict-device-guard-policy-configuration-part-1-device-drivers-fd1a281b35a8) + [Config](https://gist.github.com/mattifestation/72fe5c0eb36598186b995c5781d4198b)
* [Updating Device Guard Code Integrity Policies ](http://www.exploit-monday.com/2016/12/updating-device-guard-code-integrity.html)
* https://github.com/mattifestation/WDACTools
* https://www.sixdub.net/?p=367
* https://dfir-blog.com/2016/01/03/protecting-windows-networks-applocker/
* http://www.scip.ch/en/?labs.20150507
* https://www.sysadmins.lv/blog-en/powershell-50-and-applocker-when-security-doesnt-mean-security.aspx
* https://insights.sei.cmu.edu/cert/2016/06/bypassing-application-whitelisting.html
* http://www.howtogeek.com/howto/8739/restrict-users-to-run-only-specified-programs-in-windows-7/
* https://www.bleepingcomputer.com/tutorials/create-an-application-whitelist-policy-in-windows/
* https://www.asd.gov.au/publications/protect/Hardening_Win7_SP1.pdf       13. list of path to whitelist
* https://github.com/MHaggis/hunt-detect-prevent/blob/master/Prevention/applocker.md
* https://github.com/MHaggis/hunt-detect-prevent/blob/master/Prevention/Applocker/applocker.xml
* https://github.com/iadgov/AppLocker-Guidance/tree/master/AppLocker%20Starter%20Policy
* https://www.sans.org/reading-room/whitepapers/Whitelists/finding-evil-whitelist-35832
* https://helgeklein.com/blog/2012/08/applocker-solutions-to-common-problems/  UAC=NoAppLocker group
* https://hinchley.net/articles/an-approach-for-managing-microsoft-applocker-policies/
* https://mssec.wordpress.com/2015/10/22/applocker-bypass-checker/
* https://posts.specterops.io/assessing-the-effectiveness-of-hash-based-application-whitelisting-blacklist-rules-fe1cb01975a9


## License

BSD 2-clause

