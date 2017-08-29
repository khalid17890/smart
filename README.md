# smartbroker

Build an intelligent broker using the same MQTT algorithm, creating a new one around it. 
The published messages arriving at the broker processing is based on rules engine before getting forwarded to the subscribers (Example: Multiple temperature sensors publishing temperature every second, which is being received by the Broker.
Instead of the normal message forwarding the broker is able to perform actions like averaging the temperature, and that averaged temp should be highlighted on the subscribed clients).
The rules(to average) should be changed through web interface, where a track record of incoming messages are kept.

Sorting of sensors values and types in database is done with the help of payload which could be in JSON format.
The payload in JSON format from temp sensor {"d":{"CPUTemp":24.546}​} and the from the payload it is identified what type of sensor is that (can include ID in the payload as well).

--- mqtt topics
smartbroker/aggregator/log (json)
  aggregator node writes log to it, in form of
  {origin: ..., msg: ..., severity: ..., timestamp: ...}
smartbroker/aggregator/config (json)
  holds current configuration; this is written by web application 
  and read by Aggregator Node
smartbroker/wakeup (1)
  all components listen to this
smartbroker/second_timer (1)
  gives tick once per second
smartbroker/sensors/temperature//status (0..1)
  all sensors report their status here
smartbroker/sensors/temperature//measurement (number)
  all sensors report their readings here
smartbroker/heaters//status (0..1)
  all heaters report their online/offline status here
smartbroker/heaters//running (0..1)
  all heaters are controlled by this
smartbroker/ep/component//output/ (json)
  all EP Components send their Output Slot data through here
smartbroker/ep/component//input/ (json)
  EP Components read their input from here

--- problems encountered


rapidjson: over-engineered, schema errors reported only by crash and
had to be inspected in gdb; json schema validation was not working,
and its support had to be scrapped in the project
it wasn't very obvious how to integrate SVG objects with logical
model (which is needed to create config JSON); in the end decision
was to have logical model managed by SVG object accessors and
constructors
