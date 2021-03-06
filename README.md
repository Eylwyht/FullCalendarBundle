## FullCalendarBundle

[![Build Status](https://travis-ci.org/ancarebeca/FullCalendarBundle.svg)](https://travis-ci.org/ancarebeca/FullCalendarBundle)

This bundle allow you to integrate [FullCalendar.js](http://fullcalendar.io/) library in your Symfony2.

## Requirements
* FullCalendar.js v2.3.2
* Symfony v2.3+
* PHP v5.3+
* PHPSpec 

Installation
------------
Installation process:

1. [Download FullCalendarBundle using composer](download-fullcalendarbundle)
2. [Enable bundle](enable-bundle)
3. [Create your Event class](create-event)
4. [Create and Configure your own event adapter](config-adapter)
5. [Add styles and scripts in your template](styles-scripts)
6. [Add Routing](routing)

### 1. Download FullCalendarBundle using composer <a id="download-fullcalendarbundle"></a>

```bash
$> composer require ancarebeca/full-calendar-bundle
```

### 2. Enable bundle <a id="download-fullcalendarbundle"></a>

```php
// app/AppKernel.php

public function registerBundles()
{
    return array(
        // ...
        new AncaRebeca\FullCalendarBundle\FullCalendarBundle(),
    );
}
```
### 3. Create your Calendar Event class <a id="create-event"></a>

```php
// src/AppBundle/Entity/EventCustom.php

<?php

namespace AppBundle\Entity;

use AncaRebeca\FullCalendarBundle\Model\Event as BaseEnvet;

class CalendarEvent extends BaseEnvet
{
	// Your fields 
}
```

### 4. Create and Configure your own event adapter <a id="config-adapter"></a>

```php
// src/AppBundle/Adapter/CustomAdapter.php

<?php

namespace AppBundle\Adapter;

use AncaRebeca\FullCalendarBundle\Adapter\CalendarAdapterInterface;
use AncaRebeca\FullCalendarBundle\Model\EventInterface;
use AppBundle\Entity\CustomAdapter;

class CustomAdapter implements CalendarAdapterInterface
{
    /**
     * @param \Datetime $startDate
     * @param \Datetime $endDate
     *
     * @return EventInterface[]
     */
    public function getData(\Datetime $startCalendarDate, \Datetime $endCalendarDate)
    {
    	 // You may want do a custom query to populate the events	
        return [
            new CalendarEvent('Event Title 1', new \DateTime()),
            new CalendarEvent('Event Title 2', new \DateTime())
        ];
    }
}
```

Adding bundle config:

```yaml
// app/config/config.yml

full_calendar:
    adapter_class: AppBundle\Adapter\CustomAdapter
    serializer_class: # by defualt AncaRebeca\FullCalendarBundle\Service\Serializer
```

### 5. Add styles and scripts in your template <a id="styles-scripts"></a>

Add html template to display the calendar:

```twig
{% block body %}
    {% include '@FullCalendar/Calendar/calendar.html.twig' %}
{% endblock %}
```

Add styles:

```twig
{% block stylesheets %}
    <link rel="stylesheet" href="{{ asset('bundles/fullcalendar/css/fullcalendar/fullcalendar.min.css') }}" />
{% endblock %}
```

Add javascript:

```twig
{% block javascripts %}
    <script type="text/javascript" src="{{ asset('bundles/fullcalendar/js/fullcalendar/lib/jquery.min.js') }}"></script>
    <script type="text/javascript" src="{{ asset('bundles/fullcalendar/js/fullcalendar/lib/moment.min.js') }}"></script>
    <script type="text/javascript" src="{{ asset('bundles/fullcalendar/js/fullcalendar/fullcalendar.min.js') }}"></script>
    <script type="text/javascript" src="{{ asset('bundles/fullcalendar/js/fullcalendar/fullcalendar.default-settings.js') }}"></script>
{% endblock %}
```

Install assets

```bash
$> php app/console assets:install web
```

### 6. Define routes by default <a id="routing"></a>

```yaml
# app/config/config.yml

ancarebeca_fullcalendar:
    resource: "@FullCalendarBundle/Resources/config/routing.yml"
```

# Extending Basic functionalities

## Extending the Calendar Javascript
If you want to customize the FullCalendar javascript you can copy the fullcalendar.default-settings.js in Resources/public/js, and add your own logic:

```javascript
$(function () {
	$('#calendar-holder').fullCalendar({
		header: {
		    left: 'prev, next',
		    center: 'title',
		    right: 'month, agendaWeek, agendaDay'
		},
		timezone: ('Europe/London'),
		businessHours: {
		    start: '09:00',
		    end: '17:30',
		    dow: [1, 2, 3, 4, 5]
		},
		allDaySlot: false,
		defaultView: 'agendaWeek',
		lazyFetching: true,
		firstDay: 1,
		selectable: true,
		timeFormat: {
		    agenda: 'h:mmt',
		    '': 'h:mmt'
		},
		columnFormat:{
		    month: 'ddd',
		    week: 'ddd D/M',
		    day: 'dddd'
		},
		editable: true,
		eventDurationEditable: true,
		eventSources: [
		{
			url: /your/custom/route,
			type: 'POST',
			error: function() {
			   //alert()
			}
		}
]
```

## Define your own serializer
You may want to define your on serializer. You only need to:

Create your serializer implementing the serializer class:

```php
// src/AppBundle/Serializer/CustomSerializer.php

<?php

namespace AppBundle\Serializer;

use AncaRebeca\FullCalendarBundle\Model\EventInterface;

class CustomSerializer implements SerializerInterface
{
    /**
     * @param array $data
     *
     * @return string json
     */
    public function serialize(array $data)
    {
		 // your code
		  
        return $json;
    }
}
```
Adding config:

```yaml
// app/config/config.yml

full_calendar:
    adapter_class: AppBundle\Adapter\CustomAdapter
    serializer_class: AppBundle\Serializer\CustomSerializer
```

Contribute and feedback
-------------------------

Any feedback and contribution will be very appreciated.