---
layout: splash
title: "Empowering data-driven decisions with a unique blend of finance and data science expertise."
header:
  overlay_color: "#555"
  overlay_image: /assets/images/home_page/data_science_finance.jpg
  overlay_filter: 0.7
  cta_label: "View My Portfolio"
  cta_url: /portfolio/
  cta_classes: "button button-primary"
  # actions:
  #  - label: "Download My Resume"
  #    url: "#resume-placeholder"
  #    class: "button button-outline"
# excerpt: "Empowering data-driven decisions with a unique blend of finance and data science expertise."
---
## Professional Summary
Welcome! I'm Timothy Robbins, CPA, a seasoned revenue accounting professional with over 25 years of experience specializing in ASC 606 compliance and revenue recognition for SaaS and technology companies. I excel in automating accounting processes, leading ERP implementations, and ensuring financial compliance in fast-paced, high-tech environments.

### Data Science Certification
In March 2024, I earned the **Certified Data Science Professional in Python** certification from DataCamp. This certification, equivalent to 2 years of Python programming experience, validates my proficiency in Python for data science, including hands-on experience with key tools and techniques in the field. It marks my formal transition into data science, building upon my extensive experience in finance and accounting.

Combining my deep understanding of finance with my data science expertise, I bring a unique skill set that enables me to bridge the gap between these two critical areas, ultimately driving value through data-driven insights and strategies.

## Explore My Work
- **[Portfolio Projects](/portfolio/)**: A showcase of projects demonstrating practical applications of data science across various sectors.
- **[Detailed Projects](/posts/)**: In-depth case studies and discussions on data-driven solutions in business contexts.
- **[About Me](/about/)**: A narrative of my career evolution and key experiences that shaped my expertise.

## Featured Projects
<div class="projects-grid">
  {% for post in site.portfolio %}
    {% if post.featured %}
      <div class="project-item">
        <a href="{{ post.url }}" class="image-effect-container">
          <img src="{{ post.header.teaser | default: '/assets/images/placeholder.jpg' }}" alt="{{ post.title }}">
          <div class="overlay">
            <h3>{{ post.title }}</h3>
          </div>
        </a>
        <p>{{ post.excerpt }}</p>
      </div>
    {% endif %}
  {% endfor %}
</div>

## Contact Me
<div class="contact-form">
  <form action="https://formspree.io/f/xrbzlpdq" method="POST">
    <label for="name">Your Name:</label>
    <input type="text" id="name" name="name" required>

    <label for="email">Your Email:</label>
    <input type="email" id="email" name="email" required>

    <label for="message">Your Message:</label>
    <textarea id="message" name="message" rows="5" required></textarea>

    <button type="submit">Send</button>
  </form>