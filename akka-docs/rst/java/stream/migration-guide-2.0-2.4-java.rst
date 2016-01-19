.. _migration-streams-2.0-2.4-java:

##############################
Migration Guide 2.0.x to 2.4.x
##############################

General notes
=============

Changed Operators
=================

``expand()`` is now based on an Iterator
----------------------------------------

Previously the ``expand`` combinator required two functions as input: the first
one lifted incoming values into an extrapolation state and the second one
extracted values from that, possibly evolving that state. This has been
simplified into a single function that turns the incoming element into an
Iterator.

The most prominent use-case previously was to just repeat the previously received value::

    // This no longer works!
    Flow.of(Integer.class).expand(i -> i)(i -> new Pair<>(i, i)); 

In Akka 2.4.x this is simplified to:

.. includecode:: ../code/docs/stream/MigrationsJava.java#expand-continually

If state needs to be be kept during the expansion process then this state will
need to be managed by the Iterator. The example of counting the number of
expansions might previously have looked like::

    // This no longer works!
    Flow.of(Integer.class).expand(i -> new Pair<>(i, 0))(
      pair -> new Pair<>(new Pair<>(pair.first(), pair.second()),
                         new Pair<>(pair.first(), pair.second() + 1)));

In Akka 2.4.x this is formulated like so:

.. includecode:: ../code/docs/stream/MigrationsJava.java#expand-state

