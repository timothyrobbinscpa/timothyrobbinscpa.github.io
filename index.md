---
layout: splash
title: "Tim Robbins: Bridging the GAP between Finance and Data Science"
header:
  overlay_color: "#333"
  overlay_image: /assets/images/home_page/data_science_finance.jpg
  overlay_filter: 0.7
  cta_label: "View My Portfolio"
  cta_url: /portfolio/
  cta_classes: "button button-primary"
  # actions:
  #  - label: "Download My Resume"
  #    url: "#resume-placeholder"
  #    class: "button button-outline"
excerpt: "Empowering data-driven decisions with a unique blend of finance and data science expertise."
---

## About Me
I’m Tim Robbins, a data scientist with a rich background in finance. My transition from a CPA to a data scientist has allowed me to develop a unique perspective on financial data, helping businesses make informed, data-driven decisions. I specialize in leveraging machine learning and advanced analytics to solve complex business problems.

## Data Science Certification
**Certified Data Science Professional in Python**  
*DataCamp - March 2024*  
This certification, which is equivalent to 2 years of Python programming experience, validates my proficiency in Python for data science, including hands-on experience with key tools and techniques in the field. This certification marks my formal transition into data science, building upon my extensive experience in finance and accounting.

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