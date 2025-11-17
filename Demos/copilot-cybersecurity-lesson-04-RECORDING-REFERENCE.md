# Lesson 4 Recording Quick Reference Card

**Print this and keep it next to your recording setup** 📹

---

## ⏱️ 40-Minute Timeline

| Time | Slide | Content | Notes |
|------|-------|---------|-------|
| 0:00-2:00 | 1-2 | Introduction + Learning objectives | Energy high, set expectations |
| 2:00-12:00 | 3-4 | Security code review concept + DEMO | Show Copilot Chat finding JWT issues |
| 12:00-22:00 | 5-6 | Compliance reporting + DEMO | GHAS → OWASP report → Executive summary |
| 22:00-32:00 | 7-8 | Custom linters + DEMO | Build Semgrep rule for Azure credentials |
| 32:00-38:00 | 9-10 | Dependency management + DEMO | CVE exploitability analysis → Auto PR |
| 38:00-40:00 | 11 | Key takeaways | Celebrate wins, tease Lesson 5 |

---

## 🎯 Demo Environment Checklist

### Before Recording Starts
- [ ] VS Code open with Copilot Chat panel visible
- [ ] GitHub CLI authenticated (`gh auth status`)
- [ ] Azure CLI authenticated (`az account show`)
- [ ] Demo repository cloned: `copilot-security-demos/lesson-04`
- [ ] Dependencies installed: `npm install` in all demo folders
- [ ] Semgrep installed: `semgrep --version`
- [ ] Browser tab open: GitHub repository security tab
- [ ] Clean Copilot Chat history (start fresh)
- [ ] Terminal font size readable in recording (18pt+)

### Test Before Recording
```bash
# Quick smoke test
cd demos/auth-api && npm test
cd ../vulnerable-app && npm audit
semgrep --version
gh api user
```

---

## 💬 Critical Prompts (Copy-Paste Ready)

### Demo 1: Security Code Review
```
I need a security code review of this Express.js API. The architecture:

- Express REST API with JWT authentication
- Role-based access control (admin, user roles)
- User profile endpoints with PII
- Deployed to Azure App Service with internet access
- Processes sensitive user data (email, profile info)

Focus on authentication and authorization vulnerabilities. Use OWASP Top 10 as framework.
```

### Demo 2: STRIDE Threat Model
```
Generate a STRIDE threat model for this API architecture. Consider:
- API is internet-accessible
- Uses JWT bearer token authentication
- Stores user PII in MongoDB
- Deployed as single Azure App Service instance

Provide specific attack scenarios for each STRIDE category.
```

### Demo 3: Custom Semgrep Rule
```
Generate a Semgrep rule that detects hardcoded Azure Storage connection strings in JavaScript and TypeScript code.

The rule should:
- Match string literals containing 'AccountName=' and 'AccountKey='
- Match both single and double quotes
- Detect the pattern in variable assignments and function arguments
- Provide a clear error message explaining the security risk
- Suggest using DefaultAzureCredential instead
- Include severity level: HIGH
- Map to CWE-798 (Use of Hard-coded Credentials)

Output in Semgrep YAML format.
```

### Demo 4: CVE Exploitability Analysis
```
Analyze CVE-2024-5678 (axios SSRF vulnerability) for exploitability in our context.

ARCHITECTURE CONTEXT:
- We use axios for backend API calls to trusted internal microservices
- All axios requests go to URLs constructed from environment variables
- No user input is directly passed to axios URL construction
- Application runs in Azure App Service with network isolation
- Network security groups restrict outbound traffic to known services

QUESTIONS:
1. Is this CVE exploitable in our architecture?
2. What is the attack path (if exploitable)?
3. What is the business impact if exploited?
4. Recommended priority: Critical/High/Medium/Low?
5. Can we delay remediation or must we patch immediately?
```

---

## 🎤 Tim's Voice Reminders

### Opening Hooks (Use These)
- "Here's the thing about [concept]..."
- "Real talk:"
- "Let's get hands-on with..."
- "This is where it gets interesting..."

### Teaching Moments
- "The beauty of this approach..."
- "Think of this as..."
- "Bottom line:"
- "I learned this the hard way when..."

### Avoid These Phrases
- ❌ "As you can see..." (accessibility issue)
- ❌ "Obviously..." (dismissive)
- ❌ "Simply..." or "Just..." (minimizes complexity)
- ❌ Generic "This is cool" without explaining WHY

### Signature Phrases
- "Force multiplier"
- "Defense-in-depth"
- "Attack surface"
- "Making Copilot your security tool instead of your security problem"

---

## 📈 Key Metrics to Mention

- **10x faster** security code reviews using Copilot Chat
- **55% faster** code velocity with Copilot (GitHub 2024 survey)
- **85% reduction** in vulnerability backlog (Contoso example)
- **67% decrease** in vulnerability count over 6 months (Tailwind Traders)
- **80% fewer** repeat violations with educational linter messages
- **40% auto-merge** rate for low-risk dependency updates

---

## 🏢 Enterprise Company Names (Rotate Usage)

1. **Adventure Works** - E-commerce platform
2. **Contoso** - SaaS company
3. **Fabrikam** - Financial services
4. **Tailwind Traders** - Retail/supply chain
5. **Wide World Importers** - Manufacturing
6. **Northwind** - Healthcare/medical

**Pattern:** "[Company] uses [technology] to [solve problem]. Result: [metric]."

---

## ⚠️ Common Recording Issues

| Issue | Solution |
|-------|----------|
| Copilot response too verbose | Say: "Can you make that more concise?" |
| Security analysis too generic | Add more architectural context to prompt |
| Semgrep rule syntax error | Have corrected version ready in `linters/` folder |
| GitHub API rate limit | Use cached results from `ghas-findings.json` |
| Demo script fails | Have pre-recorded terminal output as backup |

---

## ✅ Recording Day Checklist

### Morning Of
- [ ] Coffee ☕ (seriously)
- [ ] Test all demo environments
- [ ] Clear Copilot Chat history
- [ ] Close unnecessary applications (email, Slack, etc.)
- [ ] Set phone to Do Not Disturb
- [ ] Verify screen recording software working
- [ ] Check audio levels (speak at normal volume)

### During Recording
- [ ] Smile when you talk (even if not on camera - it affects voice)
- [ ] Pause 2 seconds after each slide transition
- [ ] Show the tool working, don't just describe it
- [ ] Celebrate small wins ("Perfect! Look at that.")
- [ ] If you make a mistake, pause 5 seconds and restart sentence

### After Each Demo
- [ ] Save Copilot Chat history (export to file)
- [ ] Screenshot successful outputs
- [ ] Note any deviations from script
- [ ] Drink water 💧

---

## 🎓 Teaching Philosophy Reminders

**Feynman Technique:**
1. Explain like the student knows NOTHING about AI security
2. Use simple analogies (preferably music/guitar references)
3. Build complexity gradually
4. Test understanding with questions

**Bridge Builder Philosophy:**
- Connect legacy Windows security mindset to modern cloud
- Traditional pentesting → AI-assisted security analysis
- Manual code review → Conversational security discovery
- Annual audits → Continuous compliance automation

**Skeptical Optimism:**
- AI accelerates discovery, doesn't replace judgment
- Automation is a force multiplier, not autopilot
- Always validate AI findings with architectural context
- Security is still fundamentally about thinking like an attacker

---

## 🚀 Energy & Enthusiasm

**High-Energy Moments:**
- Learning objectives (set expectations)
- Each demo introduction ("Let's see this in action!")
- When Copilot finds a critical vulnerability ("This is exactly what we're looking for!")
- Key takeaways (celebrate what students learned)

**Reflective Moments:**
- Discussing exploitability analysis (nuanced thinking)
- Business impact assessment (connect to real-world consequences)
- Pro tips (share hard-won experience)

**Pacing:**
- Speak 10% slower than normal conversation
- Pause after important points
- Don't rush through code samples - let them sink in

---

## 📞 If Something Goes Wrong

**Plan B for Each Demo:**

1. **Security Code Review:** Pre-saved Copilot Chat conversation in `demos/auth-api/chat-backup.md`
2. **Compliance Report:** Pre-generated report in `scripts/example-report.md`
3. **Custom Linter:** Working Semgrep rule in `linters/azure-storage-auth.yml`
4. **Dependency Management:** Sample PR at github.com/techtrainertim/copilot-security-demos/pull/42

**Emergency Restart:**
- Take a 5-second pause
- Say: "Let me show you that again..."
- Execute from Plan B backup

---

## 🎬 Final Pre-Recording Checklist

- [ ] Read through all speaker notes one time
- [ ] Test each demo command once
- [ ] Verify Copilot Chat is responsive
- [ ] Check that all links work (timw.info/copilot-security)
- [ ] Set up water/coffee within reach
- [ ] Deep breath - you've got this! 🚀

---

**Remember:** You're Tim Warner. You've built 200+ Pluralsight courses. You've trained 1M+ learners. This is just another day at the office - except today, you're teaching the future of AI-assisted security. Make it count.

**End with:** "All course materials available at timw.info/copilot-security. See you in Lesson 5!"
