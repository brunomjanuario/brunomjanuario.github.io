---
title: Résumé
icon: fas fa-user-tie
order: 1
toc: true
---

<style>
.rz { --rz-accent: #2a9d8f; }
html[data-mode="dark"] .rz { --rz-accent: #4dd0c4; }

.rz .rz-header {
  display: flex; flex-wrap: wrap; align-items: baseline;
  gap: .4rem 1rem; margin-bottom: .25rem;
}
.rz .rz-role {
  font-size: 1.05rem; font-weight: 600; color: var(--rz-accent);
  letter-spacing: .3px; text-transform: uppercase; margin-bottom: .6rem;
}
.rz .rz-contact {
  display: flex; flex-wrap: wrap; gap: .5rem .9rem;
  font-size: .9rem; margin: .2rem 0 1.4rem;
}
.rz .rz-contact a { text-decoration: none; }
.rz .rz-contact i { color: var(--rz-accent); width: 1.1em; text-align: center; }

.rz h2 {
  border-bottom: 2px solid var(--rz-accent);
  padding-bottom: .3rem; margin-top: 2.2rem;
}

/* Experience timeline */
.rz .rz-job {
  border-left: 2px solid var(--card-border-color, rgba(128,128,128,.25));
  padding: 0 0 1.4rem 1.2rem; margin-left: .3rem; position: relative;
}
.rz .rz-job::before {
  content: ""; position: absolute; left: -7px; top: .35rem;
  width: 12px; height: 12px; border-radius: 50%;
  background: var(--rz-accent); box-shadow: 0 0 0 3px var(--main-bg, #fff);
}
.rz .rz-job:last-child { padding-bottom: .3rem; }
.rz .rz-title { font-size: 1.05rem; font-weight: 600; margin-bottom: .1rem; }
.rz .rz-meta {
  font-size: .85rem; color: var(--text-muted-color, #888);
  margin-bottom: .55rem;
}
.rz .rz-meta .rz-dates { float: right; }
.rz .rz-job ul { margin-bottom: 0; padding-left: 1.1rem; }
.rz .rz-job li { margin-bottom: .3rem; }

/* Skills + tags */
.rz .rz-tags { display: flex; flex-wrap: wrap; gap: .45rem; margin: .3rem 0 1rem; }
.rz .rz-tag {
  font-size: .82rem; padding: .22rem .7rem; border-radius: 999px;
  background: color-mix(in srgb, var(--rz-accent) 14%, transparent);
  border: 1px solid color-mix(in srgb, var(--rz-accent) 35%, transparent);
  white-space: nowrap;
}
.rz .rz-skillgroup { margin-bottom: 1rem; }
.rz .rz-skillgroup h4 { font-size: .95rem; margin-bottom: .4rem; }

.rz .rz-edu { margin-bottom: .9rem; }
.rz .rz-edu .rz-title { font-weight: 600; }
.rz .rz-edu .rz-meta { margin-bottom: 0; }

@media (max-width: 576px) {
  .rz .rz-meta .rz-dates { float: none; display: block; }
}
</style>

<div class="rz" markdown="0">

<div class="rz-role">Software Developer · Backend &amp; Full-Stack</div>

<div class="rz-contact">
  <span><i class="fas fa-map-marker-alt"></i> Porto, Portugal</span>
  <a href="mailto:bruno.januario1998@gmail.com"><i class="fas fa-envelope"></i> bruno.januario1998@gmail.com</a>
  <a href="https://www.linkedin.com/in/brunojanuario/" target="_blank" rel="noopener"><i class="fab fa-linkedin"></i> LinkedIn</a>
  <a href="https://github.com/brunomjanuario" target="_blank" rel="noopener"><i class="fab fa-github"></i> GitHub</a>
</div>

<p>
Software Developer with 3+ years of international experience delivering high-impact
solutions across the <strong>automotive</strong>, <strong>e-commerce</strong>, and
<strong>finance</strong> sectors. Proficient in <strong>Java, Kotlin, Spring Boot,
and AWS</strong>, with strong expertise in designing scalable microservices and
backend systems. Currently working on a banking platform for <strong>BNP
Paribas</strong>, following an e-commerce role at Mercedes-Benz IO where I led a
leasing and financing calculator that reduced bug tickets by 80%. I bring a
proactive, quality-driven approach and thrive in cross-functional teams focused on
delivering real value.
</p>

<h2>Experience</h2>

<div class="rz-job">
  <div class="rz-title">Full-Stack Developer · BNP Paribas</div>
  <div class="rz-meta">HN Services Portugal <span class="rz-dates">2025 – Present</span></div>
  <ul>
    <li>Full-stack development on a banking platform for BNP Paribas, working across backend services and web front end.</li>
    <li>Backend built with <strong>Java</strong>, <strong>Spring</strong>, and <strong>Hibernate</strong>, backed by a <strong>Sybase</strong> database.</li>
    <li>Developed and maintained front-end features with <strong>ExtJS</strong>, keeping them aligned with backend services.</li>
  </ul>
</div>

<div class="rz-job">
  <div class="rz-title">Backend Developer · Mercedes-Benz IO</div>
  <div class="rz-meta">Deloitte Portugal <span class="rz-dates">Apr 2024 – 2025</span></div>
  <ul>
    <li>Building a high-traffic e-commerce platform serving millions of daily users, focused on the leasing &amp; financing calculator.</li>
    <li>Designed and implemented a scalable Backend-for-Frontend (BFF) in <strong>Spring Boot</strong> and <strong>Kotlin</strong>.</li>
    <li>Reduced bug tickets by <strong>80%</strong> through clean architecture, rigorous testing, and proactive monitoring with <strong>Datadog</strong>.</li>
    <li>Strengthened CI/CD pipelines and introduced robust automated testing to boost reliability and delivery speed.</li>
  </ul>
</div>

<div class="rz-job">
  <div class="rz-title">Full-Stack Developer · Schaeffler</div>
  <div class="rz-meta">Deloitte Portugal <span class="rz-dates">May 2023 – Apr 2024</span></div>
  <ul>
    <li>Brought in to accelerate a delayed project; led end-to-end delivery of a web application for the automotive parts industry, shipping all features on time.</li>
    <li>Built the backend with <strong>Java Spring Boot</strong> and <strong>PostgreSQL</strong>, designing scalable schemas for complex data.</li>
    <li>Developed a responsive <strong>Angular</strong> frontend focused on UX and accessibility, and architected the full system across backend, frontend, and database layers.</li>
    <li>Deployed and maintained the solution on <strong>Microsoft Azure</strong>.</li>
  </ul>
</div>

<div class="rz-job">
  <div class="rz-title">Full-Stack Developer · BMW</div>
  <div class="rz-meta">Deloitte Portugal <span class="rz-dates">Jan 2022 – May 2023</span></div>
  <ul>
    <li>Corrective and evolutive maintenance on a complex vehicle production and distribution simulation system.</li>
    <li>Designed and optimized microservices with <strong>J2EE, Spring, PostgreSQL, OracleDB</strong>, and <strong>Apache Kafka</strong> for real-time data streaming.</li>
    <li>Managed cloud infrastructure and deployments with <strong>Docker</strong> and <strong>AWS</strong> (ECR, ECS, RDS, Lambda) for scalable, resilient operations.</li>
    <li>Implemented monitoring and alerting tooling and aligned services with frontend teams' UI needs.</li>
  </ul>
</div>

<h2>Education</h2>

<div class="rz-edu">
  <div class="rz-title">Postgraduate Diploma in IT for Organizations</div>
  <div class="rz-meta">ISCTE – Instituto Universitário de Lisboa · Final classification: 17/20 <span class="rz-dates">2024 – 2025</span></div>
</div>

<div class="rz-edu">
  <div class="rz-title">Bachelor's Degree in Sports Science</div>
  <div class="rz-meta">University of Porto <span class="rz-dates">2016 – 2019</span></div>
</div>

<h2>Certifications</h2>

<ul>
  <li><strong>AWS Certified Solutions Architect – Associate</strong> (2023)</li>
  <li><strong>AWS Certified Cloud Practitioner</strong> (2022)</li>
  <li><strong>CS50x</strong> – Harvard University (2022)</li>
  <li><strong>Full-Stack Bootcamp</strong> – Code for All_ (2021)</li>
</ul>

<h2>Skills</h2>

<div class="rz-skillgroup">
  <h4>Languages</h4>
  <div class="rz-tags">
    <span class="rz-tag">Java</span>
    <span class="rz-tag">Kotlin</span>
    <span class="rz-tag">TypeScript</span>
    <span class="rz-tag">SQL</span>
  </div>
</div>

<div class="rz-skillgroup">
  <h4>Frameworks &amp; Libraries</h4>
  <div class="rz-tags">
    <span class="rz-tag">Spring Boot</span>
    <span class="rz-tag">Spring</span>
    <span class="rz-tag">J2EE</span>
    <span class="rz-tag">Hibernate</span>
    <span class="rz-tag">Angular</span>
    <span class="rz-tag">ExtJS</span>
  </div>
</div>

<div class="rz-skillgroup">
  <h4>Cloud &amp; DevOps</h4>
  <div class="rz-tags">
    <span class="rz-tag">AWS</span>
    <span class="rz-tag">Azure</span>
    <span class="rz-tag">Docker</span>
    <span class="rz-tag">Jenkins</span>
    <span class="rz-tag">CI/CD</span>
    <span class="rz-tag">Datadog</span>
  </div>
</div>

<div class="rz-skillgroup">
  <h4>Data &amp; Messaging</h4>
  <div class="rz-tags">
    <span class="rz-tag">PostgreSQL</span>
    <span class="rz-tag">OracleDB</span>
    <span class="rz-tag">MySQL</span>
    <span class="rz-tag">Sybase</span>
    <span class="rz-tag">Apache Kafka</span>
  </div>
</div>

<div class="rz-skillgroup">
  <h4>Tools &amp; Build</h4>
  <div class="rz-tags">
    <span class="rz-tag">Git</span>
    <span class="rz-tag">Maven</span>
    <span class="rz-tag">Gradle</span>
    <span class="rz-tag">Atlassian Suite</span>
  </div>
</div>

<h2>Languages</h2>

<div class="rz-tags">
  <span class="rz-tag">Portuguese — Native</span>
  <span class="rz-tag">English — Advanced</span>
</div>

</div>
