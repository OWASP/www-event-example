---

title: Schedule & Trainings
layout: event_noheader
permalink: /trainings/

---

# {{ page.title }}

{% assign trainings = site.data.trainings | sort: 'Title' %}
{% for trainer in trainings %}
<section class="trainer-section" id="{{trainer.SectionId}}">
<hr>
<ul>
<li><h3 class='training-header'>{{ trainer.Title }}<button class="cta-button grey" onclick="location.href='{{trainer.URL}}';" style="margin-left:1em;cursor: pointer;max-width=80px;">Register</button></h3></li>
<li class="training-desc">{{ trainer.Description }}</li>
    <ul>
        {% for tr in trainer.Trainers %}
        <li style="font-size:smaller;"><hr><div class="training-container"><div class="training-image" style="background-image:url('{{tr.Image}}');"></div><div class='trainer-container'><a href="/trainers/#{{tr.TrainerId}}">{{tr.Name}}</a></div></div><div class='trainer-container-mobile'><a href="/trainers/#{{trainer.TrainerId}}">{{tr.Name}}</a></div>{{tr.Biography}}</li>
        {% endfor %}
    </ul>
</ul>
</section>
{% endfor %}
