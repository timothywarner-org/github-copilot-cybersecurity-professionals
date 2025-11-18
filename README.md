# GitHub Copilot for Cybersecurity Professionals

**A comprehensive video training course on securing code with AI-assisted development**

by Tim Warner | Microsoft Press | 3.5 hours of hands-on training

---

## 📚 Course Overview

If you're writing code, GitHub Copilot is already in your workflow. If you're responsible for security, that fact should concern you and excite you in equal measure. Copilot generates code fast—but it's also generating vulnerabilities at the same velocity: SQL injection, XSS, weak cryptography, misconfigured access controls.

**This course isn't about banning Copilot. It's about making it your security tool instead of your security problem.**

You'll learn how to use GitHub Copilot to:
- **Detect vulnerabilities** in your codebase
- **Build hardened infrastructure** with secure-by-default patterns
- **Automate security testing** end-to-end
- **Audit code and dependencies** systematically
- **Prove compliance** without drowning in manual work

**Five lessons. 200 minutes. Real patterns you can ship today.**

---

## 🎯 What You'll Learn

### Lesson 1: Vulnerability Detection with Copilot (40 min)
- Configure Copilot for security tasks and secure coding best practices
- Identify and mitigate SQL injection vulnerabilities in code
- Detect and prevent XSS vulnerabilities with Copilot assistance
- Create custom Copilot-assisted vulnerability scanners for proprietary code

### Lesson 2: Implement Security Protocols (40 min)
- Build secure authentication and authorization systems
- Implement encryption and secure key management
- Create secure API gateway authentication and enforce least privilege access controls
- Design zero-trust network access policies and enforce segmentation rules using IaC

### Lesson 3: Automated Security Testing (40 min)
- Generate AI-assisted security unit tests for critical functions
- Create fuzz testing harnesses with Copilot for input validation
- Automate DAST and SAST workflows for web and cloud applications
- Build continuous security validation pipelines in CI/CD

### Lesson 4: Security Code Review, Threat Modeling, and Auditing (40 min)
- Use Copilot Chat to assist in secure code reviews and threat modeling
- Automatically generate security review checklists and risk assessment reports
- Create custom security linters and static analysis rules for detecting misconfigurations
- Automate dependency vulnerability assessments and patching workflows

### Lesson 5: Compliance, Incident Response, and Configuration Management (40 min)
- Generate compliant infrastructure-as-code templates and security baselines
- Automate CIS and NIST benchmark verification scripts
- Build STIG compliance validation and auto-remediation tools
- Automate security documentation, audit logs, and incident response playbooks with AI

---

## ⚡ Quick Start

### Prerequisites

**Required Knowledge:**
- Basic programming experience (JavaScript, Python, or Java)
- Familiarity with version control (Git/GitHub)
- Basic understanding of web application security concepts
- Experience with command-line interfaces

**Required Tools:**
- **GitHub Account** with GitHub Copilot enabled ([Get Copilot](https://github.com/features/copilot))
- **VS Code** (latest version) with GitHub Copilot extension
- **Git** (2.x or later)
- **Docker Desktop** (for running vulnerable applications)
- **Node.js** (18.x or later) and npm
- **Python** (3.9 or later)
- **Java** (JDK 17 or later) and Maven
- **Terraform** (1.5 or later)

**System Requirements:**
- 8 GB RAM minimum (16 GB recommended)
- 10 GB free disk space
- macOS, Windows 10/11, or Linux

### Environment Setup (5 minutes)

1. **Clone this repository:**
   ```bash
   git clone https://github.com/timothywarner-org/github-copilot-cybersecurity-professionals.git
   cd github-copiolot-cybersecurity-professionals
   ```

2. **Verify prerequisites:**
   ```bash
   # Check versions
   git --version
   node --version
   python --version
   java -version
   terraform -version
   docker --version
   ```

3. **Set up vulnerable applications:**
   ```bash
   # NodeGoat (Node.js)
   cd vulnerable-apps/NodeGoat
   npm install
   npm start
   # Access at http://localhost:4000

   # WebGoat (Java)
   cd vulnerable-apps/WebGoat
   mvn clean install
   mvn spring-boot:run
   # Access at http://localhost:8080/WebGoat

   # TerraGoat (Terraform)
   cd vulnerable-apps/TerraGoat
   terraform init
   # Ready for scanning

   # PyGoat (Python)
   cd vulnerable-apps/PyGoat
   pip install -r requirements.txt
   python manage.py runserver
   # Access at http://localhost:8000
   ```

4. **Configure GitHub Copilot in VS Code:**
   - Install GitHub Copilot extension
   - Sign in with your GitHub account
   - Enable Copilot Chat
   - Verify with: Press `Ctrl+I` (Windows/Linux) or `Cmd+I` (macOS)

---

## 📂 Repository Structure

```
github-copiolot-cybersecurity-professionals/
├── README.md                    # This file - start here!
├── Demos/                       # Demo runbooks for each lesson
│   ├── Lesson-01-Vulnerability-Detection-Demo-Runbook.md
│   ├── Lesson-02-Security-Protocols-Demo-Runbook.md
│   ├── Lesson-03-Automated-Security-Testing-Demo-Runbook.md
│   ├── Lesson-04-Code-Review-Threat-Modeling-Demo-Runbook.md
│   ├── Lesson-05-Compliance-Configuration-Demo-Runbook.md
│   └── lesson-01-quick-reference.md  # Quick reference cards
├── Docs/                        # Course documentation
│   ├── Warner.HighLevelOutline.GitHub Copilot for Cybersecurity Specialists.md
│   └── GitHub_Copilot_Cybersecurity_Specialists_Scripts.md
├── PPTs/                        # PowerPoint presentations
│   ├── copilot-cybersecurity-lesson-01-complete.pptx
│   ├── copilot-cybersecurity-lesson-02-v2.pptx
│   ├── copilot-cybersecurity-lesson-03.pptx
│   ├── copilot-cybersecurity-lesson-04-enriched.pptx
│   └── copilot-cybersecurity-lesson-05.pptx
├── vulnerable-apps/             # Intentionally vulnerable applications
│   ├── NodeGoat/               # OWASP Node.js vulnerable app
│   ├── WebGoat/                # OWASP Java vulnerable app
│   ├── TerraGoat/              # Bridgecrew Terraform vulnerable IaC
│   └── PyGoat/                 # OWASP Python vulnerable app
└── LICENSE                      # MIT License
```

---

## 🎓 How to Use This Course

### For Students

**Recommended Learning Path:**

1. **Watch the video lesson** for each module (40 minutes each)
2. **Open the corresponding demo runbook** in `/Demos/`
3. **Follow along hands-on** using the vulnerable applications
4. **Practice with Copilot** using the prompts demonstrated
5. **Review the quick reference** cards for key concepts
6. **Repeat for each lesson** in sequence

**Self-Paced Learning:**
- Each lesson is self-contained but builds on previous lessons
- Budget 1-2 hours per lesson including hands-on practice
- Complete all 5 lessons in 1-2 weeks for best retention

**Getting Help:**
- Review the demo runbook's "Common Pitfalls" sections
- Check the vulnerable app documentation in each subdirectory
- Refer to the course scripts in `/Docs/` for detailed explanations

### For Instructors

**Teaching Preparation:**

1. **Review the demo runbook** for your lesson
2. **Read the quick reference card** (when available) for pacing guidance
3. **Set up all vulnerable applications** before recording/teaching
4. **Test each Copilot prompt** to account for response variations
5. **Review speaker notes** in the PowerPoint presentations

**Demo Delivery Tips:**
- Use the runbooks as your script - they include teaching points, expected outputs, and transitions
- The PowerPoint speaker notes use the FRAMER methodology (Frame, Relate, Apply, Metaphor, Expand, Reinforce)
- Enterprise examples use fictional companies: Contoso, Fabrikam, Adventure Works, Tailwind Traders, Wide World Importers, Northwind
- PRO TIP sections on every slide provide actionable security advice

**Recording Guidelines:**
- Allocate 40 minutes per lesson
- Use the quick reference cards for energy/pacing checkpoints
- Account for Copilot latency in your timing
- Have backup screenshots ready in case Copilot responses vary

---

## 🗺️ Lesson Navigation Guide

### Lesson 1: Vulnerability Detection with Copilot

**Demo Runbook:** `/Demos/Lesson-01-Vulnerability-Detection-Demo-Runbook.md`
**Quick Reference:** `/Demos/lesson-01-quick-reference.md`
**Primary App:** NodeGoat (`vulnerable-apps/NodeGoat`)
**Key Topics:** SQL injection, XSS, custom scanners, CodeQL

**Start here if you want to:**
- Learn to find vulnerabilities in generated code
- Build custom security scanners with Copilot
- Understand SQL injection and XSS detection patterns

### Lesson 2: Implement Security Protocols

**Demo Runbook:** `/Demos/Lesson-02-Security-Protocols-Demo-Runbook.md`
**Primary Apps:** WebGoat, PyGoat, TerraGoat
**Key Topics:** Authentication, OAuth, encryption, zero-trust, IaC security

**Start here if you want to:**
- Build secure authentication systems (OAuth 2.0, PKCE)
- Implement encryption and key management
- Design zero-trust network policies with Terraform

### Lesson 3: Automated Security Testing

**Demo Runbook:** `/Demos/Lesson-03-Automated-Security-Testing-Demo-Runbook.md`
**Primary App:** NodeGoat
**Key Topics:** Security unit tests, fuzzing, SAST/DAST, CI/CD pipelines

**Start here if you want to:**
- Generate security tests with Copilot
- Build fuzz testing harnesses
- Automate CodeQL and OWASP ZAP in CI/CD

### Lesson 4: Security Code Review, Threat Modeling, and Auditing

**Demo Runbook:** `/Demos/Lesson-04-Code-Review-Threat-Modeling-Demo-Runbook.md`
**Primary Apps:** WebGoat, NodeGoat
**Key Topics:** Threat modeling (STRIDE), code review, linters, dependency scanning

**Start here if you want to:**
- Use Copilot for threat modeling
- Generate security review checklists
- Automate dependency vulnerability assessments

### Lesson 5: Compliance, Incident Response, and Configuration Management

**Demo Runbook:** `/Demos/Lesson-05-Compliance-Configuration-Demo-Runbook.md`
**Primary App:** TerraGoat
**Key Topics:** CIS Benchmarks, NIST, STIG, IaC compliance, incident response playbooks

**Start here if you want to:**
- Generate compliant infrastructure-as-code
- Automate CIS/NIST/STIG compliance checks
- Build incident response playbooks with AI

---

## 🔧 Vulnerable Applications Guide

This course uses four intentionally vulnerable applications for hands-on demos:

### NodeGoat (Primary for Lessons 1, 3, 4)
**Tech:** Node.js, Express, MongoDB
**Port:** 4000
**Use Cases:** Web vulnerabilities (SQLi, XSS), SAST/DAST, dependency scanning
**Setup:** `cd vulnerable-apps/NodeGoat && npm install && npm start`

### WebGoat (Primary for Lessons 2, 4)
**Tech:** Java, Spring Boot
**Port:** 8080
**Use Cases:** Enterprise auth, JWT, threat modeling, Spring Security patterns
**Setup:** `cd vulnerable-apps/WebGoat && mvn spring-boot:run`

### TerraGoat (Primary for Lessons 2, 5)
**Tech:** Terraform (AWS, Azure, GCP)
**Use Cases:** IaC security, cloud misconfigurations, zero-trust, compliance
**Setup:** `cd vulnerable-apps/TerraGoat && terraform init`
**Note:** Does not provision real cloud resources - used for static analysis only

### PyGoat (Supporting for Lesson 2)
**Tech:** Python, Django
**Port:** 8000
**Use Cases:** Python-specific vulnerabilities, Django auth patterns
**Setup:** `cd vulnerable-apps/PyGoat && pip install -r requirements.txt && python manage.py runserver`

---

## 🚀 Getting Started Checklist

Use this checklist to verify you're ready to start the course:

- [ ] GitHub Copilot enabled and working in VS Code
- [ ] Git installed and configured
- [ ] Docker Desktop running
- [ ] Node.js 18+ and npm installed
- [ ] Python 3.9+ installed
- [ ] Java JDK 17+ installed
- [ ] Terraform 1.5+ installed
- [ ] NodeGoat running on port 4000
- [ ] WebGoat running on port 8080
- [ ] PyGoat running on port 8000
- [ ] TerraGoat initialized (terraform init)
- [ ] All demo runbooks accessible in `/Demos/`
- [ ] PowerPoint presentations accessible in `/PPTs/`

**Troubleshooting:**
- **Port conflicts:** Change ports in app configs or stop conflicting services
- **Docker issues:** Ensure Docker Desktop is running and you have permissions
- **npm/Maven errors:** Clear caches (`npm cache clean --force`, `mvn clean`)
- **Copilot not responding:** Check GitHub Copilot status in VS Code status bar

---

## 📖 Additional Resources

### Course Materials
- **Course Repository:** [github.com/timothywarner-org/github-copilot-cybersecurity-professionals](https://github.com/timothywarner-org/github-copilot-cybersecurity-professionals)
- **Course Website:** timw.info/copilot-security

### GitHub Copilot Documentation
- [GitHub Copilot Docs](https://docs.github.com/en/copilot)
- [Copilot for Business Security](https://resources.github.com/security/github-copilot-enterprise-security/)
- [GitHub Advanced Security](https://docs.github.com/en/code-security)

### Security Frameworks & Standards
- [OWASP Top 10 (2021)](https://owasp.org/www-project-top-ten/)
- [CWE Top 25 Most Dangerous Software Weaknesses](https://cwe.mitre.org/top25/)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks)
- [DISA STIGs](https://public.cyber.mil/stigs/)

### Vulnerable Applications
- [OWASP NodeGoat](https://github.com/OWASP/NodeGoat)
- [OWASP WebGoat](https://github.com/WebGoat/WebGoat)
- [Bridgecrew TerraGoat](https://github.com/bridgecrewio/terragoat)
- [OWASP PyGoat](https://github.com/adeyosemanputra/pygoat)

### Security Tools
- [GitHub CodeQL](https://codeql.github.com/)
- [OWASP ZAP](https://www.zaproxy.org/)
- [Checkov (IaC Scanner)](https://www.checkov.io/)
- [Trivy (Container Scanner)](https://trivy.dev/)
- [Semgrep](https://semgrep.dev/)

---

## 🎯 Learning Objectives Mapped to Lessons

| Learning Objective | Lesson | Time |
|--------------------|--------|------|
| Configure Copilot for security tasks | 1.1 | 8 min |
| Detect SQL injection vulnerabilities | 1.2 | 10 min |
| Prevent XSS attacks | 1.3 | 10 min |
| Build custom vulnerability scanners | 1.4 | 12 min |
| Implement secure authentication (OAuth) | 2.1 | 10 min |
| Manage encryption and keys | 2.2 | 10 min |
| Create API gateway auth | 2.3 | 10 min |
| Design zero-trust network policies | 2.4 | 10 min |
| Generate security unit tests | 3.1 | 10 min |
| Create fuzz testing harnesses | 3.2 | 10 min |
| Automate DAST/SAST workflows | 3.3 | 10 min |
| Build CI/CD security pipelines | 3.4 | 10 min |
| Conduct secure code reviews | 4.1 | 10 min |
| Generate security checklists | 4.2 | 10 min |
| Create custom security linters | 4.3 | 10 min |
| Automate dependency scanning | 4.4 | 10 min |
| Generate compliant IaC templates | 5.1 | 10 min |
| Automate CIS/NIST benchmarks | 5.2 | 10 min |
| Validate STIG compliance | 5.3 | 10 min |
| Automate security documentation | 5.4 | 10 min |

**Total Course Duration:** 3 hours 30 minutes (210 minutes)

---

## 💡 Course Philosophy

> "We're not teaching people to fear AI or ban Copilot. We're showing security professionals how to channel Copilot toward security work. Every lesson should leave students feeling empowered with reusable patterns they can ship today. Make it real, make it practical, make it matter."

**Core Message:** AI tools are force multipliers for security teams, not replacements. The combination of your security expertise plus Copilot's pattern recognition creates something more powerful than either alone.

---

## 🤝 Contributing

This is a course repository, not an open-source project, but we welcome:
- **Bug reports** for demo runbook errors
- **Suggestions** for additional examples or scenarios
- **Tool compatibility notes** for different versions

Please open an issue with your findings.

---

## 📄 License

This repository is licensed under the MIT License. See [LICENSE](LICENSE) for details.

**Note:** The vulnerable applications in `/vulnerable-apps/` retain their original licenses:
- NodeGoat: Apache 2.0
- WebGoat: Apache 2.0
- TerraGoat: Apache 2.0
- PyGoat: MIT

---

## 📞 About the Author

**Tim Warner** is a Microsoft MVP, Pluralsight author, and cybersecurity instructor with over 25 years of IT experience. He specializes in cloud security, DevSecOps, and security automation.

- **Website:** [TechTrainerTim.com](https://techtrainertim.com)
- **Twitter/X:** [@TechTrainerTim](https://twitter.com/TechTrainerTim)
- **LinkedIn:** [timothywarner](https://linkedin.com/in/timothywarner)
- **YouTube:** [TechTrainerTim](https://youtube.com/@TechTrainerTim)

---

## 🎉 Ready to Start?

1. **Complete the setup checklist** above
2. **Start with Lesson 1** demo runbook
3. **Follow along hands-on** with NodeGoat
4. **Practice with Copilot** using the demonstrated prompts
5. **Ship secure code faster** than ever before

**Let's turn GitHub Copilot into your security multiplier. Let's begin.**

---

*Course Version: 1.0 | Last Updated: November 2025*
