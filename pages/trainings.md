---

title: Training Courses
layout: event_noheader
permalink: /program/trainings/

---

# {{page.title}}
<br>
#### Training subject to change based on trainer availability.

{% assign tpricing = site.data.pricing | where: 'title', 'Training Courses' %}
Training prices are {{ tpricing[0].price }}

<section class='training'>
{% for training_type in site.data.trainings %}
<h3 style="background-color: #{{ training_type.color }};">{{ training_type.title }}</h3>
<div class="tr-mobile-table" style="border-left-color: #{{ training_type.color }} !important;">
    {% for class in training_type.classes %}
    <div class="class-container">
        <div class="class-title"><a href="{{ class.url }}">{{ class.title }}</a></div>
        <div><strong>Days</strong>: {{ class.days }}</div>
        <div><strong>Instructors</strong>: {{ class.trainer }}</div>
        <div class="class-description">{{ class.description }}</div>
    </div>
    <hr>
    {% endfor %}
</div>
{% endfor %}
</section>

