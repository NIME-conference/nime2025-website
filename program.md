---
layout: page  
title: Program
description: The program of events for NIME2025.
permalink: /program/
---

{: .info-box}
A list of all accepted papers, music, and workshops is listed [here by track]({% link program-by-track.md %}) and [here by session]({% link program-by-session.md %}).


{% assign sorted_sessions = site.data.sessions | sort: "start" %}

<script>
  var calendarEvents = (function () {
    var sessions = {{ sorted_sessions | jsonify }};
    var typeColours = {
      admin:         '#2e312d',
      papers:        '#7e7a72',
      posters:       '#8f95a5',
      installations: '#97a7b6',
      concert:       '#565b68',
      workshops:     '#5f6e62',
      plenary:       '#b69255'
    };
    return sessions.map(function (session) {
      var colour = typeColours[session.type];
      return Object.assign({}, session, {
        url: '{{ site.baseurl }}/sessions/' + session.id + '.html',
        backgroundColor: colour,
        borderColor: colour
      });
    });
  })();
</script>

{% capture fullcalendar_src %}{{ '/assets/imports/fullcalendar@6.1.17/index.global.min.js' | relative_url }}{% endcapture %}
{% include calendar-timezone-picker.html
   default_timezone="Australia/Canberra"
   default_timezone_label="Australia / Canberra (Default)"
   fullcalendar_src=fullcalendar_src %}

<h2>Sessions</h2>


<div class="row row-cols-1 row-cols-md-3 row-cols-sm-2 g-4">
  {% for session in sorted_sessions %}
    {% capture session_url %}{{ session.id | datapage_url: "sessions" | relative_url }}{% endcapture %}
    <div class="col">
      <div class="card h-100">
        {% if session.image_url %}
          <img src="{{ session.image_url | relative_url }}" class="card-img-top" alt="{{ session.title }}">
        {% endif %}
        <div class="card-body">
          <h5 class="card-title">
            <a href="{{ session_url }}" class="text-decoration-none text-dark">{{ session.title }}</a>
          </h5>
          <h6 class="card-subtitle mb-2 text-muted">{{ session.type | capitalize }} Session</h6>
          <p class="card-text">
            <strong>Date:</strong> {{ session.date | date: "%A, %B %d, %Y" }}<br>
            <strong>Time:</strong> {{ session.date | date: "%I:%M %p" }} AEST<br>
            <strong>Location:</strong> {{ session.location }}<br>
          </p>
        </div>
        <div class="card-footer">
          <a href="{{ session_url }}" class="btn btn-outline-secondary">Details</a>
          {% if session.video_url %}
            <a href="{{ session.video_url }}" class="btn btn-outline-secondary" target="_blank">Video</a>
          {% endif %}
        </div>
      </div>
    </div>
  {% endfor %}
</div>