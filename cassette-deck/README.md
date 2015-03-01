# Cassette Deck

Rewind your application!

*Note: This would also be great to have as an npm or meteor package.*

## Introduction

Cassette Deck allows you to move the state of your application back and forth in time as if it had the buttons on a cassette deck.

Cassette Deck depends on your application implementing the [Event Sourcing](http://martinfowler.com/eaaDev/EventSourcing.html) pattern, having an Event Store and an Event Bus. You will probably want to use the Cassette Deck on a [Parallel Model](http://martinfowler.com/eaaDev/ParallelModel.html) of your production application, and not on the actual production application itself. You can use it to figure out how your application got into an awkward state, run queries on alternate states of your application or to prepare a [Retroactive Event](http://martinfowler.com/eaaDev/RetroactiveEvent.html).

## Setting up

Your Cassette Deck will require a Cassette (an event store) to read and a Stereo (an event bus) system to publish to. You will first have to implement these interfaces, and then tell the cassette deck which Cassette and which Stereo to use.

```php
class EventStore implements Cassette
```

```php
class EventBus implements Stereo
```

```php
$cassette = new EventStore();
$stereo = new EventBus();

$deck = new CassetteDeck\Deck();
$deck->mount($cassette);
$deck->connect($stereo);
```

Start playing and enjoy the tunes!

## Usage

#### Rewind to start

`$ php deck rewind`

#### Fast-forward to now

`$ php deck fast-forward`

#### Show deck time

`$ php deck time`

#### Show next event

`$ php deck event`

#### Move one event ahead or back

`$ php deck forward`
`$ php deck back`

#### Move multiple events

`$ php deck forward --steps="5"`
`$ php deck back --steps="5"`

This is the same as:

`$ php deck forward --steps="events:5"`
`$ php deck back --steps="events:5"`

#### Using time intervals

`$ php deck back --steps="milliseconds:5"`
`$ php deck back --steps="seconds:5"`
`$ php deck back --steps="minutes:5"`
`$ php deck back --steps="hours:5"`
`$ php deck back --steps="days:5"`
`$ php deck back --steps="weeks:5"`
`$ php deck back --steps="months:5"`
`$ php deck back --steps="years:5"`



## PHP API

Although most users will want to use Cassette Deck through the command line, the same API is also accessible in PHP.

#### Rewind to start

```php
$deck->rewind();
```

#### Fast-forward to now

```php
$deck->fastForward();
```

#### Move one event ahead or back

```php
$deck->forward();
$deck->back();
```

#### Move multiple events

```php
$deck->forward(5);
$deck->back(5);
```

This is the same as:

```php
$deck->forward(5, "events");
$deck->back(5, "events");
```

#### Using time intervals

```php
$deck->back(3, "milliseconds");
$deck->back(3, "seconds");
$deck->back(3, "minutes");
$deck->back(3, "days");
$deck->back(3, "weeks");
$deck->back(3, "months");
$deck->back(3, "years");
```

## Source code changes

If your event store specifies a source code version in the Event Store, the Deck ask permission whenever you try to move before a version change. This will allow you to move back to the appropriate commit before rewinding further.