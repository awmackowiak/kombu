.. _changelog:

================
 Change history
================

.. _version-4.6.7:

4.6.7
=====
:release-date: 2019-12-07 20:45 A.M UTC+6:00
:release-by: Asif Saif Uddin

- Use importlib.metadata from the standard library on Python 3.8+ (#1086).
- Add peek lock settings to be changed using transport options (#1119).
- Fix redis health checks (#1122).
- Reset ready before execute callback (#1126).
- Add missing parameter queue_args in kombu.connection.SimpleBuffer (#1128) 

.. _version-4.6.6:

4.6.6
=====
:release-date: 2019-11-11 00:15 A.M UTC+6:00
:release-by: Asif Saif Uddin

- Revert _lookup_direct and related changes of redis. 
- Python 3.8 support
- Fix 'NoneType' object has no attribute 'can_read' bug of redis transport
- Issue #1019 Fix redis transport socket timeout 
- Add wait timeout settings to receive queue message (#1110)
- Bump py-amqp to 2.5.2 

.. _version-4.6.5:

4.6.5
=====
:release-date: 2019-09-30 19:30 P.M UTC+6:00
:release-by: Asif Saif Uddin

- Revert _lookup api and correct redis implemetnation. 
- Major overhaul of redis test cases by adding more full featured fakeredis module.
- Add more test cases to boost coverage of kombu redis transport.
- Refactor the producer consumer test cases to be based on original mocks and be passing
- Fix lingering line length issue in test.
- Sanitise url when include_password is false
- Pinned pycurl to 7.43.0.2 as it is the latest build with wheels provided
- Bump py-amqp to 2.5.2 


.. _version-4.6.4:

4.6.4
=====
:release-date: 2019-08-14 22:45 P.M UTC+6:00
:release-by: Asif Saif Uddin

- Use importlib-metadata instead of pkg_resources for better performance
- Allow users to switch URLs while omitting the resource identifier (#1032)
- Don't stop receiving tasks on 503 SQS error. (#1064) 
- Fix maybe declare (#1066)
- Revert "Revert "Use SIMEMBERS instead of SMEMBERS to check for queue (Redis Broker)
- Fix MongoDB backend to work properly with TTL (#1076)
- Make sure that max_retries=0 is treated differently than None (#1080)
- Bump py-amqp to 2.5.1 


.. _version-4.6.3:

4.6.3
=====
:release-date: 2019-06-15 12:45 A.M UTC+6:00
:release-by: Asif Saif Uddin

- Revert FastUUID for kombu 4.6


.. _version-4.6.2:

4.6.2
=====
:release-date: 2019-06-15 12:45 A.M UTC+6:00
:release-by: Asif Saif Uddin

- Fix sbugs and regressions


.. _version-4.6.1:

4.6.1
=====
:release-date: 2019-06-06 10:30 A.M UTC+6:00
:release-by: Asif Saif Uddin

- Fix some newly introduced bug in kombu 4.6

.. _version-4.6.0:

4.6.0
=====
:release-date: 2019-05-30 15:30 P.M UTC+6:00
:release-by: Asif Saif Uddin

-

.. _version-4.5.0:

4.5.0
=====
:release-date: 2019-03-3 18:30 P.M UTC+3:00
:release-by: Omer Katz

- The Redis transport now supports a custom separator for keys.

  Previously when storing a key in Redis which represents a queue
  we used the hardcored value ``\x06\x16`` separator to store
  different attributes of the queue in the queue's name.

  The separator is now configurable using the sep
  transport option:

  .. code-block:: python

    with Connection('redis://', transport_options={
            'sep': ':',
        }):
        # ...
        pass

  Contributed by **Joris Beckers**

- When the SQS server returns a timeout we ignore it and keep trying
  instead of raising an error.

  This will prevent Celery from raising an error and hanging.

  Contributed by **Erwin Rossen**

- Properly declare async support for the Qpid transport.

  If you are using this transport we strongly urge you to upgrade.

  Contributed by **Rohan McGovern**

- Revert `celery/kombu#906 <https://github.com/celery/kombu/pull/906>`_ and
  introduce unique broadcast queue names as an optional keyword argument.

  If you want each broadcast queue to have a unique name specify `unique=True`:

  .. code-block:: pycon

    >>> from kombu.common import Broadcast
    >>> q = Broadcast(queue='foo', unique=True)
    >>> q.name
    'foo.7ee1ac20-cda3-4966-aaf8-e7a3bb548688'
    >>> q = Broadcast(queue='foo')
    >>> q.name
    'foo'

- Codebase improvements and fixes by:

  - **Omer Katz**

.. _version-4.4.0:

4.4.0
=====
:release-date: 2019-03-3 9:00 P.M UTC+2:00
:release-by: Omer Katz

- Restore bz2 import checks in compression module.

  The checks were removed in `celery/kombu#938 <https://github.com/celery/kombu/pull/938>`_ due to assumption that it only affected Jython.
  However, bz2 support can be missing in Pythons built without bz2 support.

  Contributed by **Patrick Woods**

- Fix regression that occurred in 4.3.0
  when parsing  Redis Sentinel master URI containing password.

  Contributed by **Peter Lithammer**

- Handle the case when only one Redis Sentinel node is provided.

  Contributed by **Peter Lithammer**

- Support SSL URL parameters correctly for `rediss://`` URIs.

  Contributed by **Paul Bailey**

- Revert `celery/kombu#954 <https://github.com/celery/kombu/pull/954>`_.
  Instead bump the required redis-py dependency to 3.2.0
  to include this fix `andymccurdy/redis-py@4e1e748 <https://github.com/andymccurdy/redis-py/commit/4e1e74809235edc19e03edb79c97c80a3e4e9eca>`_.

  Contributed by **Peter Lithammer**

- Added support for broadcasting using a regular expression pattern
  or a glob pattern to multiple Pidboxes.

  Contributed by **Jason Held**

.. _version-4.3.0:

4.3.0
=====
:release-date: 2019-01-14 7:00 P.M UTC+2:00
:release-by: Omer Katz

- Added Python 3.7 support.

  Contributed by **Omer Katz**, **Mads Jensen** and **Asif Saif Uddin**

- Avoid caching queues which are declared with a TTL.

  Queues that are declared with a TTL are now also be excluded from the
  in-memory cache in case they expire between publishes on the same channel.

  Contributed by **Matt Yule-Bennett**

- Added an index to the Message table for the SQLAlchemy transport.

  The index allows to effectively sorting the table by the message's timestamp.

  .. note::

    We do not provide migrations for this model yet.
    You will need to add the index manually if you are already
    using the SQLAlchemy transport.

    The syntax may vary between databases.
    Please refer to your database's documentation for instructions.

  Contributed by **Mikhail Shcherbinin**

- Added a timeout that limits the amount of time we retry
  to reconnect to a transport.

  Contributed by **:github_user:`tothegump`**

- :class:``celery.asynchronous.hub.Hub`` is now reentrant.

  This allows calling :func:`celery.bin.celery.main` to revive a worker in
  the same process after rescuing from shutdown (:class:``SystemExit``).

  Contributed by **Alan Justino da Silva**

- Queues now accept string exchange names as arguments as documented.

  Tests were added to avoid further regressions.

  Contributed by **Antonio Gutierrez**

- Specifying names for broadcast queues now work as expected.

  Previously, named broadcast queues did not create multiple queues per worker.
  They incorrectly declared the named queue which resulted in one queue per
  fanout exchange, thus missing the entire point of a fanout exchange.
  The behavior is now matched to unnamed broadcast queues.

  Contributed by **Kuan Hsuan-Tso**

- When initializing the Redis transport in conjunction with gevent
  restore all unacknowledged messages to queue.

  Contributed by **Gal Cohen**

- Allow :class:``kombu.simple.SimpleQueue`` to pass queue_arguments to Queue object.

  This allows :class:``kombu.simple.SimpleQueue`` to connect to RabbitMQ queues with
  custom arguments like 'x-queue-mode'='lazy'.

  Contributed by **C Blue Neeh**

- Add support for 'rediss' scheme for secure Redis connections.

  The rediss scheme defaults to the least secure form, as
  there is no suitable default location for `ca_certs`. The recommendation
  would still be to follow the documentation and specify `broker_use_ssl` if
  coming from celery.

  Contributed by **Daniel Blair**

- Added the Azure Storage Queues transport.

  The transport is implemented on top of Azure Storage
  Queues. This offers a simple but scalable and low-cost PaaS
  transport for Celery users in Azure. The transport is intended to be
  used in conjunction with the Azure Block Blob Storage backend.

  Contributed by **Clemens Wolff**, **:github_user:`@ankurokok`**,
  **Denis Kisselev**, **Evandro de Paula**, **Martin Peck**
  and **:github_user:`@michaelperel`**

- Added the Azure Service Bus transport.

  The transport is implemented on top of Azure Service Bus and
  offers PaaS support for more demanding Celery workloads in Azure.
  The transport is intended to be used in conjunction with the Azure
  CosmosDB backend.

  Contributed by **Clemens Wolff**, **:github_user:`@ankurokok`**,
  **Denis Kisselev**, **Evandro de Paula**, **Martin Peck**
  and **:github_user:`@michaelperel`**

- Drop remaining mentions of Jython support completely.

  Contributed by **Asif Saif Uddin** and **Mads Jensen**

- When publishing messages to the Pidbox, retry if an error occurs.

  Contributed by **Asif Saif Uddin**

- Fix infinite loop in :method:``kombu.asynchronous.hub.Hub.create_loop``.

  Previous attempt to fix the problem (PR kombu/760) did not consider
  an edge case. It is now fixed.

  Contributed by **Vsevolod Strukchinsky**

- Worker shutdown no longer duplicates messages when using the SQS broker.

  Contributed by **Mintu Kumar Sah**

- When using the SQS broker, prefer boto's default region before our hardcoded default.

  Contributed by **Victor Villas**

- Fixed closing of shared redis sockets which previously caused Celery to hang.

  Contributed by **Alexey Popravka**

- the `Pyro`_ transport (:mod:`kombu.transport.pyro`) now works with
  recent Pyro versions. Also added a Pyro Kombu Broker that this transport
  needs for its queues.

  Contributed by **Irmen de Jong**

- Handle non-base64-encoded SQS messages.

  Fix contributed by **Tim Li**, **Asif Saif Uddin** and **Omer Katz**.

- Move the handling of Sentinel failures to the redis library itself.

  Previously, Redis Sentinel worked only if the first node's sentinel
  service in the URI was up. A server outage would have caused downtime.

  Contributed by **Brian Price**

- When using Celery and the pickle serializer with binary data as part of the
  payload, `UnicodeDecodeError` would be raised as the content was not utf-8.
  We now replace on errors.

  Contributed by **Jian Dai**

- Allow setting :method:``boto3.sqs.create_queue`` Attributes via transport_options.

  Contributed by **Hunter Fernandes**

- Fixed infinite loop when entity.channel is replaced by revive() on connection
  drop.

  Contributed by **Tzach Yarimi**

- Added optional support for Brotli compression.

  Contributed by **Omer Katz**

- When using the SQS broker, FIFO queues with names that ended with the 'f' letter
  were incorrectly parsed. This is now fixed.

  Contributed by **Alex Vishnya** and **Ilya Konstantinov**

-  Added optional support for LZMA compression.

  Contributed by **Omer Katz**

- Added optional support for ZStandard compression.

  Contributed by **Omer Katz**

- Require py-amqp 2.4.0 as the minimum version.

  Contributed by **Asif Saif Uddin**

- The value of DISABLE_TRACEBACKS environment variable is now respected on debug, info
  and warning logger level.

  Contributed by **Ludovic Rivallain**

- As documented in kombu/#741 and eventlet/eventlet#415
  there is a mismatch between the monkey-patched eventlet queue
  and the interface Kombu is expecting.
  This causes Celery to crash when the `broker_pool_limit`
  configuration option is set
  eventlet/eventlet#415 suggests that the mutex can be a noop.
  This is now the case.

  Contributed by **Josh Morrow**

- Codebase improvements and fixes by:

  - **Omer Katz**
  - **Mads Jensen**
  - **Asif Saif Uddin**
  - **Lars Rinn**

- Documentation improvements by:

  - **Jon Dufresne**
  - **Fay Cheng**
  - **Asif Saif Uddin**
  - **Kyle Verhoog**
  - **Noah Hall**
  - **:github_user:`brabiega`**

.. _version-4.2.2-post1:

4.2.2-post1
===========
:release-date: 2019-01-01 04:00 P.M IST
:release-by: Omer Katz

.. note::

  The previous release contained code from master.
  It is now deleted from PyPi.
  Please use this release instead.

- No changes since previous release.

.. _version-4.2.2:

4.2.2
=====
:release-date: 2018-12-06 04:30 P.M IST
:release-by: Omer Katz

- Support both Redis client version 2.x and version 3.x.

  Contributed by **Ash Berlin-Taylor** and **Jeppe Fihl-Pearson**

.. _version-4.2.1:

4.2.1
=====
:release-date: 2018-05-21 09:00 A.M IST
:release-by: Omer Katz

.. note::

  The 4.2.0 release contained remains of the ``async`` module by accident.
  This is now fixed.

- Handle librabbitmq fileno raising a ValueError when socket is not connected.

  Contributed by **Bryan Shelton**

.. _version-4.2.0:

4.2.0
=====
:release-date: 2018-05-21 09:00 A.M IST
:release-by: Omer Katz

- Now passing ``max_retries``, ``interval_start``, ``interval_step``,
  ``interval_max`` parameters from broker ``transport_options`` to
  :meth:`~kombu.Connection.ensure_connection` when returning
  :meth:`~kombu.Connection.default_connection` (Issue #765).

    Contributed by **Anthony Lukach**.

- Qpid: messages are now durable by default

    Contributed by **David Davis**

- Kombu now requires version 2.10.4 or greater of the redis library,
  in line with Celery

    Contributed by **Colin Jeanne**

- Fixed ImportError in some environments with outdated simplejson

    Contributed by **Aaron Morris**

- MongoDB: fixed failure on MongoDB versions with an "-rc" tag

    Contributed by **dust8**

- Ensure periodic polling frequency does not exceed timeout in
  virtual transport

    Contributed by **Arcadiy Ivanov**

- Fixed string handling when using python-future module

    Contributed by **John Koehl"

- Replaced "async" with "asynchronous" in preparation for Python 3.7

    Contributed by **Thomas Achtemichuk**

- Allow removing pool size limit when in use

    Contributed by **Alex Hill**

- Codebase improvements and fixes by:

    - **j2gg0s**
    - **Jon Dufresne**
    - **Jonas Lergell**
    - **Mads Jensen**
    - **Nicolas Delaby**
    - **Omer Katz**

- Documentation improvements by:

    - **Felix Yan**
    - **Harry Moreno**
    - **Mads Jensen**
    - **Omer Katz**
    - **Radha Krishna. S.**
    - **Wojciech Matyśkiewicz**

.. _version-4.1.0:

4.1.0
=====
:release-date: 2017-07-17 04:45 P.M MST
:release-by: Anthony Lukach

- SQS: Added support for long-polling on all supported queries. Fixed bug
  causing error on parsing responses with no retrieved messages from SQS.

    Contributed by **Anthony Lukach**.

- Async hub: Fixed potential infinite loop while performing todo tasks
  (Issue celery/celery#3712).

- Qpid: Fixed bug where messages could have duplicate ``delivery_tag``
  (Issue #563).

    Contributed by **bmbouter**.

- MongoDB: Fixed problem with using ``readPreference`` option at pymongo 3.x.

    Contributed by **Mikhail Elovskikh**.

- Re-added support for :pypi:``SQLAlchemy``

    Contributed by **Amin Ghadersohi**.

- SQS: Fixed bug where hostname would default to ``localhost`` if not specified
  in settings.

    Contributed by **Anthony Lukach**.

- Redis: Added support for reading password from transport URL (Issue #677).

    Contributed by **George Psarakis**.

- RabbitMQ: Ensured safer encoding of queue arguments.

    Contributed by **Robert Kopaczewski**.

- Added fallback to :func:``uuid.uuid5`` in :func:``generate_oid`` if
  :func:``uuid.uuid3`` fails.

    Contributed by **Bill Nottingham**.

- Fixed race condition and innacurrate timeouts for
  :class:``kombu.simple.SimpleBase`` (Issue #720).

    Contributed by **c-nichols**.

- Zookeeper: Fixed last chroot character trimming

    Contributed by **Dima Kurguzov**.

- RabbitMQ: Fixed bug causing an exception when attempting to close an
  already-closed connection (Issue #690).

    Contributed by **eavictor**.

- Removed deprecated use of StopIteration in generators and invalid regex
  escape sequence.

    Contributed by **Jon Dufresne**.

- Added Python 3.6 to CI testing.

    Contributed by **Jon Dufresne**.

- SQS: Allowed endpoint URL to be specified in the boto3 connection.

    Contributed by **georgepsarakis**.

- SQS: Added support for Python 3.4.

    Contributed by **Anthony Lukach**.

- SQS: ``kombu[sqs]`` now depends on :pypi:`boto3` (no longer using
  :pypi:`boto)`.

    - Adds support for Python 3.4+
    - Adds support for FIFO queues (Issue #678) and (Issue celery/celery#3690)
    - Avoids issues around a broken endpoints file (Issue celery/celery#3672)

    Contributed by **Mischa Spiegelmock** and **Jerry Seutter**.

- Zookeeper: Added support for delaying task with Python 3.

    Contributed by **Dima Kurguzov**.

- SQS: Fixed bug where :meth:`kombu.transport.SQS.drain_events` did not support
  callback argument (Issue #694).

    Contributed by **Michael Montgomery**.

- Fixed bug around modifying dictionary size while iterating over it
  (Issue #675).

    Contributed by **Felix Yan**.

- etcd: Added handling for :exc:`EtcdException` exception rather than
  :exc:`EtcdError`.

    Contributed by **Stephen Milner**.

- Documentation improvements by:

    - **Mads Jensen**
    - **Matias Insaurralde**
    - **Omer Katz**
    - **Dmitry Dygalo**
    - **Christopher Hoskin**

.. _version-4.0.2:

4.0.2
=====
:release-date: 2016-12-15 03:31 P.M PST
:release-by: Ask Solem

- Now depends on :mod:`amqp` 2.1.4

    This new version takes advantage of TCP Keepalive settings on Linux,
    making it better at detecting closed connections, also in failover
    conditions.

- Redis: Priority was reversed so, e.g. priority 0 became priority 9.

.. _version-4.0.1:

4.0.1
=====
:release-date: 2016-12-07 06:00 P.M PST
:release-by: Ask Solem

- Now depends on :mod:`amqp` 2.1.3

    This new version takes advantage of the new ``TCP_USER_TIMEOUT`` socket option
    on Linux.

- Producer: Fixed performance degradation when default exchange specified
  (Issue #651).

- QPid: Switch to using getattr in qpid.Transport.__del__ (Issue #658)

    Contributed by **Patrick Creech**.

- QPid: Now uses monotonic time for timeouts.

- MongoDB: Fixed compatibility with Python 3 (Issue #661).

- Consumer: ``__exit__`` now skips cancelling consumer if connection-related
  error raised (Issue #670).

- MongoDB: Removes use of natural sort (Issue #638).

    Contributed by **Anton Chaporgin**.

- Fixed wrong keyword argument ``channel`` error (Issue #652).

    Contributed by **Toomore Chiang**.

- Safe argument to ``urllib.quote`` must be bytes on Python 2.x (Issue #645).

- Documentation improvements by:

    - **Carlos Edo**
    - **Cemre Mengu**

.. _version-4.0:

4.0
===
:release-date: 2016-10-28 16:45 P.M UTC
:release-by: Ask Solem

- Now depends on :mod:`amqp` 2.0.

    The new py-amqp version have been refactored for better performance,
    using modern Python socket conventions, and API consistency.

- No longer depends on :mod:`anyjson`.

    Kombu will now only choose between :pypi:`simplejson` and the built-in
    :mod:`json`.

    Using the latest version of simplejson is recommended:

    .. code-block:: console

        $ pip install -U simplejson

- Removed transports that are no longer supported in this version:

    - Django ORM transport
    - SQLAlchemy ORM transport
    - Beanstalk transport
    - ZeroMQ transport
    - amqplib transport (use pyamqp).

- API Changes

    * Signature of :class:`kombu.Message` now takes body as first argment.

        It used to be ``Message(channel, body=body, **kw)``, but now it's
        ``Message(body, channel=channel, **kw)``.

        This is unlikey to affect you, as the Kombu API does not have
        users instantiate messages manually.

- New SQS transport

    Donated by NextDoor, with additional contributions from mdk.

    .. note::

        ``kombu[sqs]`` now depends on :pypi:`pycurl`.

- New Consul transport.

    Contributed by **Wido den Hollander**.

- New etcd transport.

    Contributed by **Stephen Milner**.

- New Qpid transport.

    It was introduced as an experimental transport in Kombu 3.0, but is now
    mature enough to be fully supported.

    Created and maintained by **Brian Bouterse**.

- Redis: Priority 0 is now lowest, 9 is highest.
  (**backward incompatible**)

    This to match how priorities in AMQP works.

    Fix contributed by **Alex Koshelev**.

- Redis: Support for Sentinel

    You can point the connection to a list of sentinel URLs like:

    .. code-block:: text

        sentinel://0.0.0.0:26379;sentinel://0.0.0.0:26380/...

    where each sentinel is separated by a `;`. Multiple sentinels are handled
    by :class:`kombu.Connection` constructor, and placed in the alternative
    list of servers to connect to in case of connection failure.

   Contributed by **Sergey Azovskov**, and **Lorenzo Mancini**

- RabbitMQ Queue Extensions

    New arguments have been added to :class:`kombu.Queue` that lets
    you directly and conveniently configure the RabbitMQ queue extensions.

    - ``Queue(expires=20.0)``

        Set queue expiry time in float seconds.

        See :attr:`kombu.Queue.expires`.

    - ``Queue(message_ttl=30.0)``

        Set queue message time-to-live float seconds.

        See :attr:`kombu.Queue.message_ttl`.

    - ``Queue(max_length=1000)``

        Set queue max length (number of messages) as int.

        See :attr:`kombu.Queue.max_length`.

    - ``Queue(max_length_bytes=1000)``

        Set queue max length (message size total in bytes) as int.

        See :attr:`kombu.Queue.max_length_bytes`.

    - ``Queue(max_priority=10)``

        Declare queue to be a priority queue that routes messages
        based on the ``priority`` field of the message.

        See :attr:`kombu.Queue.max_priority`.

- RabbitMQ: ``Message.ack`` now supports the ``multiple`` argument.

    If multiple is set to True, then all messages received before
    the message being acked will also be acknowledged.

- ``amqps://`` can now be specified to require SSL (Issue #610).

- ``Consumer.cancel_by_queue`` is now constant time.

- ``Connection.ensure*`` now raises :exc:`kombu.exceptions.OperationalError`.

    Things that can be retried are now reraised as
    :exc:`kombu.exceptions.OperationalError`.

- Redis: Fixed SSL support.

    Contributed by **Robert Kolba**.

- New ``Queue.consumer_arguments`` can be used for the ability to
  set consumer priority via ``x-priority``.

  See https://www.rabbitmq.com/consumer-priority.html

  Example:

  .. code-block:: python

        Queue(
            'qname',
            exchange=Exchange('exchange'),
            routing_key='qname',
            consumer_arguments={'x-priority': 3},
        )

- Queue/Exchange: ``no_declare`` option added (also enabled for
  internal amq. exchanges) (Issue #565).

- JSON serializer now calls ``obj.__json__`` for unsupported types.

    This means you can now define a ``__json__`` method for custom
    types that can be reduced down to a built-in json type.

    Example:

    .. code-block:: python

        class Person:
            first_name = None
            last_name = None
            address = None

            def __json__(self):
                return {
                    'first_name': self.first_name,
                    'last_name': self.last_name,
                    'address': self.address,
                }

- JSON serializer now handles datetimes, Django promise, UUID and Decimal.

- Beanstalk: Priority 0 is now lowest, 9 is highest.
  (**backward incompatible**)

    This to match how priorities in AMQP works.

    Fix contributed by **Alex Koshelev**.

- Redis: now supports SSL using the ``ssl`` argument to
  :class:`~kombu.Connection`.

- Redis: Fanout exchanges are no longer visible between vhosts,
  and fanout messages can be filtered by patterns.
  (**backward incompatible**)

    It was possible to enable this mode previously using the
    ``fanout_prefix``, and ``fanout_patterns``
    transport options, but now these are enabled by default.

    If you want to mix and match producers/consumers running different
    versions you need to configure your kombu 3.x clients to also enable
    these options:

    .. code-block:: pycon

        >>> Connection(transport_options={
            'fanout_prefix': True,
            'fanout_patterns': True,
        })

- Pidbox: Mailbox new arguments: TTL and expiry.

    Mailbox now supports new arguments for controlling
    message TTLs and queue expiry, both for the mailbox
    queue and for reply queues.

    - ``queue_expires`` (float/int seconds).
    - ``queue_ttl`` (float/int seconds).
    - ``reply_queue_expires`` (float/int seconds).
    - ``reply_queue_ttl`` (float/int seconds).

    All take seconds in int/float.

    Contributed by **Alan Justino**.

- Exchange.delivery_mode now defaults to :const:`None`, and the default
  is instead set by ``Producer.publish``.

- :class:`~kombu.Consumer` now supports a new ``prefetch_count`` argument,
  which if provided will force the consumer to set an initial prefetch count
  just before starting.

- Virtual transports now stores ``priority`` as a property, not in
  ``delivery_info``, to be compatible with AMQP.

- ``reply_to`` argument to ``Producer.publish`` can now be
  :class:`~kombu.Queue` instance.

- Connection: There's now a new method
  ``Connection.supports_exchange_type(type)`` that can be used to check if the
  current transport supports a specific exchange type.

- SQS: Consumers can now read json messages not sent by Kombu.

    Contributed by **Juan Carlos Ferrer**.

- SQS: Will now log the access key used when authentication fails.

    Contributed by **Hank John**.

- Added new :class:`kombu.mixins.ConsumerProducerMixin` for consumers that
  will also publish messages on a separate connection.

- Messages: Now have a more descriptive ``repr``.

    Contributed by **Joshua Harlow**.

- Async: HTTP client based on curl.

- Async: Now uses `poll` instead of `select` where available.

- MongoDB: Now supports priorities

    Contributed by **Alex Koshelev**.

- Virtual transports now supports multiple queue bindings.

    Contributed by **Federico Ficarelli**.

- Virtual transports now supports the anon exchange.

    If when publishing a message, the exchange argument is set to '' (empty
    string), the routing_key will be regarded as the destination queue.

    This will bypass the routing table compeltely, and just deliver the
    message to the queue name specified in the routing key.

- Zookeeper: Transport now uses the built-in suport in kazoo to handle
  failover when using a list of server names.

    Contributed by **Joshua Harlow**.

- ConsumerMixin.run now passes keyword arguments to .consume.

Deprecations and removals
-------------------------

- The deprecated method ``Consumer.add_queue_from_dict`` has been removed.

    Use instead:

    .. code-block:: python

        consumer.add_queue(Queue.from_dict(queue_name, **options))

- The deprecated function ``kombu.serialization.encode`` has been removed.

    Use :func:`kombu.serialization.dumps` instead.

- The deprecated function ``kombu.serialization.decode`` has been removed.

    Use :func:`kombu.serialization.loads` instead.

- Removed module ``kombu.syn``

    ``detect_environment`` has been moved to kombu.utils.compat

.. _version-3.0.37:

3.0.37
======
:release-date: 2016-10-06 05:00 P.M PDT
:release-by: Ask Solem

- Connection: Return value of ``.info()`` was no longer JSON serializable,
  leading to "itertools.cycle object not JSON serializable"
  errors (Issue #635).

.. _version-3.0.36:

3.0.36
======
:release-date: 2016-09-30 03:06 P.M PDT
:release-by: Ask Solem

- Connection: Fixed bug when cloning connection with alternate urls.

    Fix contributed by Emmanuel Cazenave.

- Redis: Fixed problem with unix socket connections.

    https://github.com/celery/celery/issues/2903

    Fix contributed by Raphael Michel.

- Redis: Fixed compatibility with older redis-py versions (Issue #576).

- Broadcast now retains queue name when being copied/pickled (Issue #578).

.. _version-3.0.35:

3.0.35
======
:release-date: 2016-03-22 11:22 P.M PST
:release-by: Ask Solem

- msgpack: msgpack support now requires msgpack-python > 0.4.7.

- Redis: TimeoutError was no longer handled as a recoverable error.

- Redis: Adds the ability to set more Redis connection options
  using ``Connection(transport_options={...})``.

    - ``socket_connect_timeout``
    - ``socket_keepalive`` (requires :mod:`redis-py` > 2.10)
    - ``socket_keepalive_options`` (requires :mod:`redis-py` > 2.10)

- msgpack: Fixes support for binary/unicode data

.. _version-3.0.34:

3.0.34
======
:release-date: 2016-03-03 05:30 P.M PST
:release-by: Ask Solem

- Qpid: Adds async error handling.

    Contributed by Brian Bouterse.

- Qpid: Delivery tag is now a UUID4 (Issue #563).

    Fix contributed by Brian Bouterse.

- Redis: Connection.as_uri() returned malformed URLs when the
  ``redis+socket`` scheme was ised (Issue celery/celery#2995).

- msgpack: Use binary encoding instead of utf-8 (Issue #570).

.. _version-3.0.33:

3.0.33
======
:release-date: 2016-01-08 06:36 P.M PST
:release-by: Ask Solem

- Now depends on :mod:`amqp` 1.4.9.

- Redis: Fixed problem with auxilliary connections causing the main
  consumer connection to be closed (Issue #550).

- Qpid: No longer uses threads to operate, to ensure compatibility with
  all environments (Issue #531).

.. _version-3.0.32:

3.0.32
======
:release-date: 2015-12-16 02:29 P.M PST
:release-by: Ask Solem

- Redis: Fixed bug introduced in 3.0.31 where the redis transport always
  connects to localhost, regardless of host setting.

.. _version-3.0.31:

3.0.31
======
:release-date: 2015-12-16 12:00 P.M PST
:release-by: Ask Solem

- Redis: Fixed bug introduced in 3.0.30 where socket was prematurely
  disconnected.

- Hub: Removed debug logging message: "Deregistered fd..." (Issue #549).

.. _version-3.0.30:

3.0.30
======
:release-date: 2015-12-07 12:28 A.M PST
:release-by: Ask Solem

- Fixes compatiblity with uuid in Python 2.7.11 and 3.5.1.

    Fix contributed by Kai Groner.

- Redis transport: Attempt at fixing problem with hanging consumer
  after disconnected from server.

- Event loop:
    Attempt at fixing issue with 100% CPU when using the Redis transport,

- Database transport: Fixed oracle compatiblity.

    An "ORA-00907: missing right parenthesis" error could manifest when using
    an Oracle database with the database transport.

    Fix contributed by Deepak N.

- Documentation fixes

    Contributed by Tommaso Barbugli.

.. _version-3.0.29:

3.0.29
======
:release-date: 2015-10-26 11:10 A.M PDT
:release-by: Ask Solem

- Fixed serialization issue for ``bindings.as_dict()`` (Issue #453).

    Fix contributed by Sergey Tikhonov.

- Json serializer wrongly treated bytes as ``ascii``, not ``utf-8``
  (Issue #532).

- MongoDB: Now supports pymongo 3.x.

    Contributed by Len Buckens.

- SQS: Tests passing on Python 3.

    Fix contributed by Felix Yan

.. _version-3.0.28:

3.0.28
======
:release-date: 2015-10-12 12:00 PM PDT
:release-by: Ask Solem

.. admonition:: Django transport migrations.

    If you're using Django 1.8 and have already created the
    kombu_transport_django tables, you have to run a fake initial migration:

    .. code-block:: console

        $ python manage.py migrate kombu_transport_django --fake-initial

- No longer compatible with South by default.

    To keep using kombu.transport.django with South migrations
    you now need to configure a new location for the kombu migrations:

    .. code-block:: python

        SOUTH_MIGRATION_MODULES = {
            'kombu_transport_django':
                'kombu.transport.django.south_migrations',
        }

- Keep old South migrations in ``kombu.transport.django.south_migrations``.

- Now works with Redis < 2.10 again.

.. _version-3.0.27:

3.0.27
======
:release-date: 2015-10-09 3:10 PM PDT
:release-by: Ask Solem

- Now depends on :mod:`amqp` 1.4.7.

- Fixed libSystem import error on some macOS 10.11 (El Capitan) installations.

    Fix contributed by Eric Wang.

- Now compatible with Django 1.9.

- Django: Adds migrations for the database transport.

- Redis: Now depends on py-redis 2.10.0 or later (Issue #468).

- QPid: Can now connect as localhost (Issue #519).

    Fix contributed by Brian Bouterse.

- QPid: Adds support for ``login_method`` (Issue #502, Issue #499).

    Contributed by Brian Bouterse.

- QPid: Now reads SASL mechanism from broker string (Issue #498).

    Fix contributed by Brian Bouterse.

- QPid: Monitor thread now properly terminated on session close (Issue #485).

    Fix contributed by Brian Bouterse.

- QPid: Fixed file descriptor leak (Issue #476).

    Fix contributed by Jeff Ortel

- Docs: Fixed wrong order for entrypoint arguments (Issue #473).

- ConsumerMixin: Connection error logs now include traceback (Issue #480).

- BaseTransport now raises RecoverableConnectionError when disconnected
  (Issue #507).

- Consumer: Adds ``tag_prefix`` option to modify how consumer tags are
  generated (Issue #509).

.. _version-3.0.26:

3.0.26
======
:release-date: 2015-04-22 06:00 P.M UTC
:release-by: Ask Solem

- Fixed compatibility with py-redis versions before 2.10.3 (Issue #470).

.. _version-3.0.25:

3.0.25
======
:release-date: 2015-04-21 02:00 P.M UTC
:release-by: Ask Solem

- pyamqp/librabbitmq now uses 5671 as default port when SSL is enabled
  (Issue #459).

- Redis: Now supports passwords in ``redis+socket://:pass@host:port`` URLs
  (Issue #460).

- ``Producer.publish`` now defines the ``expiration`` property in support
  of the `RabbitMQ per-message TTL extension`_.

    Contributed by Anastasis Andronidis.

- Connection transport attribute now set correctly for all transports.

    Contributed by Alex Koshelev.

- qpid: Fixed bug where the connectionw as not being closed properly.

    Contributed by Brian Bouterse.

- :class:`~kombu.entity.bindings` is now JSON serializable (Issue #453).

    Contributed by Sergey Tikhonov.

- Fixed typo in error when yaml is not installed (said ``msgpack``).

    Contributed by Joshua Harlow.

- Redis: Now properly handles :exc:`redis.exceptions.TimeoutError`
  raised by :mod:`redis`.

    Contributed by markow.

- qpid: Adds additional string to check for when connecting to qpid.

    When we connect to qpid, we need to ensure that we skip to the next SASL
    mechanism if the current mechanism fails. Otherwise, we will keep retrying the
    connection with a non-working mech.

    Contributed by Chris Duryee.

- qpid: Handle ``NotFound`` exceptions.

    Contributed by Brian Bouterse.

- :class:`Queue.__repr__` now makes sure return value is not unicode
  (Issue #440).

- qpid: ``Queue.purge`` incorrectly raised :exc:`AttributeErrror` if the
  does not exist (Issue #439).

    Contributed by Brian Bouterse.

- Linux: Now ignores permission errors on epoll unregister.

.. _`RabbitMQ per-message TTL extension`: https://www.rabbitmq.com/ttl.html

.. _version-3.0.24:

3.0.24
======
:release-date: 2014-11-17 11:00 P.M UTC
:release-by: Ask Solem

- The `Qpid <http://qpid.apache.org/>`_ broker is supported for Python 2.x
  environments. The Qpid transport includes full SSL support within Kombu. See
  the :mod:`kombu.transport.qpid` docs for more info.

    Contributed by Brian Bouterse and Chris Duryee through support from Red Hat.

- Dependencies: extra[librabbitmq] now requires librabbitmq 1.6.0

- Docstrings for :class:`~kombu.utils.limit.TokenBucket` did not match
  implementation.

    Fix contributed by Jesse Dhillon.

- :func:`~kombu.common.oid_from` accidentally called ``uuid.getnode()`` but
  did not use the return value.

    Fix contributed by Alexander Todorov.

- Redis: Now ignores errors when cosing the underlying connection.

- Redis: Restoring messages will now use a single connection.

- ``kombu.five.monotonic``: Can now be imported even if ctypes is not
  available for some reason (e.g. App Engine)

- Documentation: Improved example to use the ``declare`` argument to
  ``Producer`` (Issue #423).

- Django: Fixed ``app_label`` for older Django versions (``< 1.7``).
  (Issue #414).

.. _version-3.0.23:

3.0.23
======
:release-date: 2014-09-14 10:45 P.M UTC
:release-by: Ask Solem

- Django: Fixed bug in the Django 1.7 compatibility improvements related
  to autocommit handling.

    Contributed by Radek Czajka.

- Django: The Django transport models would not be created on syncdb
  after app label rename (Issue #406).

.. _version-3.0.22:

3.0.22
======
:release-date: 2014-09-04 03:00 P.M UTC
:release-by: Ask Solem

- kombu.async: Min. delay between waiting for timer was always increased to
  one second.

- Fixed bug in itermessages where message is received after the with
  statement exits the block.

    Fixed by Rumyana Neykova

- Connection.autoretry: Now works with functions missing wrapped attributes
    (``__module__``, ``__name__``, ``__doc__``).  Fixes #392.

    Contributed by johtso.

- Django: Now sets custom app label for ``kombu.transport.django`` to work
  with recent changes in Django 1.7.

- SimpleQueue removed messages from the wrong end of buffer (Issue #380).

- Tests: Now using ``unittest.mock`` if available (Issue #381).

.. _version-3.0.21:

3.0.21
======
:release-date: 2014-07-07 02:00 P.M UTC
:release-by: Ask Solem

- Fixed remaining bug in ``maybe_declare`` for ``auto_delete`` exchanges.

    Fix contributed by Roger Hu.

- MongoDB: Creating a channel now properly evaluates a connection (Issue #363).

    Fix contributed by Len Buckens.

.. _version-3.0.20:

3.0.20
======
:release-date: 2014-06-24 02:30 P.M UTC
:release-by: Ask Solem

- Reverts change in 3.0.17 where ``maybe_declare`` caches the declaration
  of auto_delete queues and exchanges.

    Fix contributed by Roger Hu.

- Redis: Fixed race condition when using gevent and the channel is closed.

    Fix contributed by Andrew Rodionoff.

.. _version-3.0.19:

3.0.19
======
:release-date: 2014-06-09 03:10 P.M UTC
:release-by: Ask Solem

- The wheel distribution did not support Python 2.6 by failing to list
  the extra dependencies required.

- Durable and auto_delete queues/exchanges can be be cached using
  ``maybe_declare``.

.. _version-3.0.18:

3.0.18
======
:release-date: 2014-06-02 06:00 P.M UTC
:release-by: Ask Solem

- A typo introduced in 3.0.17 caused kombu.async.hub to crash (Issue #360).

.. _version-3.0.17:

3.0.17
======
:release-date: 2014-06-02 05:00 P.M UTC
:release-by: Ask Solem

- ``kombu[librabbitmq]`` now depends on librabbitmq 1.5.2.

- Async: Event loop now selectively removes file descriptors for the mode
  it failed in, and keeps others (e.g read vs write).

    Fix contributed by Roger Hu.

- CouchDB: Now works without userid set.

    Fix contributed by Latitia M. Haskins.

- SQLAlchemy: Now supports recovery from connection errors.

    Contributed by Felix Schwarz.

- Redis: Restore at shutdown now works when ack emulation is disabled.

- :func:`kombu.common.eventloop` accidentally swallowed socket errors.

- Adds :func:`kombu.utils.url.sanitize_url`

.. _version-3.0.16:

3.0.16
======
:release-date: 2014-05-06 01:00 P.M UTC
:release-by: Ask Solem

- ``kombu[librabbitmq]`` now depends on librabbitmq 1.5.1.

- Redis: Fixes ``TypeError`` problem in ``unregister`` (Issue #342).

    Fix contributed by Tobias Schottdorf.

- Tests: Some unit tests accidentally required the `redis-py` library.

    Fix contributed by Randy Barlow.

- librabbitmq: Would crash when using an older version of :mod:`librabbitmq`,
  now emits warning instead.

.. _version-3.0.15:

3.0.15
======
:release-date: 2014-04-15 09:00 P.M UTC
:release-by: Ask Solem

- Now depends on :mod:`amqp` 1.4.5.

- RabbitMQ 3.3 changes QoS semantics (Issue #339).

    See the RabbitMQ release notes here:
    http://www.rabbitmq.com/blog/2014/04/02/breaking-things-with-rabbitmq-3-3/

    A new connection property has been added that can be used to detect
    whether the remote server is using this new QoS behavior:

    .. code-block:: pycon

        >>> Connection('amqp://').qos_behavior_matches_spec
        False

    so if your application depends on the old semantics you can
    use this to set the ``apply_global`` flag appropriately:

    .. code-block:: python

        def update_prefetch_count(channel, new_value):
            channel.basic_qos(
                0, new_value,
                not channel.connection.client.qos_behavior_matches_spec,
            )

- Users of :mod:`librabbitmq` is encouraged to upgrade to librabbitmq 1.5.0.

    The ``kombu[librabbitmq]`` extra has been updated to depend on this
    version.

- Pools: Now takes transport options into account when comparing connections
  (Issue #333).

- MongoDB: Fixes Python 3 compatibility.

- Async: select: Ignore socket errors when attempting to unregister handles
  from the loop.

- Pidbox: Can now be configured to use a serializer other than json,
  but specifying a serializer argument to :class:`~kombu.pidbox.Mailbox`.

    Contributed by Dmitry Malinovsky.

- Message decompression now works with Python 3.

    Fix contributed by Adam Gaca.

.. _version-3.0.14:

3.0.14
======
:release-date: 2014-03-19 07:00 P.M UTC
:release-by: Ask Solem

- **MongoDB**: Now endures a connection failover (Issue #123).

    Fix contributed by Alex Koshelev.

- **MongoDB**: Fixed ``KeyError`` when a replica set member is removed.

    Also fixes celery#971 and celery/#898.

    Fix contributed by Alex Koshelev.

- **MongoDB**: Fixed MongoDB broadcast cursor re-initialization bug.

    Fix contributed by Alex Koshelev.

- **Async**: Fixed bug in lax semaphore implementation where in
  some usage patterns the limit was not honored correctly.

    Fix contributed by Ionel Cristian Mărieș.

- **Redis**: Fixed problem with fanout when using Python 3 (Issue #324).

- **Redis**: Fixed ``AttributeError`` from attempting to close a non-existing
  connection (Issue #320).

.. _version-3.0.13:

3.0.13
======
:release-date: 2014-03-03 04:00 P.M UTC
:release-by: Ask Solem

- Redis: Fixed serious race condition that could lead to data loss.

    The delivery tags were accidentally set to be an incremental number
    local to the channel, but the delivery tags need to be globally
    unique so that a message can not overwrite an older message
    in the backup store.

    This change is not backwards incompatible and you are encouraged
    to update all your system using a previous version as soon as possible.

- Now depends on :mod:`amqp` 1.4.4.

- Pidbox: Now makes sure message encoding errors are handled by default,
  so that a custom error handler does not need to be specified.

- Redis: The fanout exchange can now use AMQP patterns to route and filter
  messages.

    This change is backwards incompatible and must be enabled with
    the ``fanout_patterns`` transport option:

    .. code-block:: pycon

        >>> conn = kombu.Connection('redis://', transport_options={
        ...     'fanout_patterns': True,
        ... })

    When enabled the exchange will work like an amqp topic exchange
    if the binding key is a pattern.

    This is planned to be default behavior in the future.

- Redis: Fixed ``cycle`` no such attribute error.

.. _version-3.0.12:

3.0.12
======
:release-date: 2014-02-09 03:50 P.M UTC
:release-by: Ask Solem

- Now depends on :mod:`amqp` 1.4.3.

- Fixes Python 3.4 logging incompatibility (Issue #311).

- Redis: Now properly handles unknown pub/sub messages.

    Fix contributed by Sam Stavinoha.

- amqplib: Fixed bug where more bytes were requested from the socket
  than necessary.

    Fix contributed by Ionel Cristian Mărieș.

.. _version-3.0.11:

3.0.11
======
:release-date: 2014-02-03 05:00 P.M UTC
:release-by: Ask Solem

- Now depends on :mod:`amqp` 1.4.2.

- Now always trusts messages of type `application/data` and `application/text`
  or which have an unspecified content type (Issue #306).

- Compression errors are now handled as decode errors and will trigger
  the ``Consumer.on_decode_error`` callback if specified.

- New ``kombu.Connection.get_heartbeat_interval()`` method that can be
  used to access the negotiated heartbeat value.

- `kombu.common.oid_for` no longer uses the MAC address of the host, but
   instead uses a process-wide UUID4 as a node id.

    This avoids a call to `uuid.getnode()` at module scope.

- Hub.add: Now normalizes registered fileno.

    Contributed by Ionel Cristian Mărieș.

- SQS: Fixed bug where the prefetch count limit was not respected.

.. _version-3.0.10:

3.0.10
======
:release-date: 2014-01-17 05:40 P.M UTC
:release-by: Ask Solem

- Now depends on :mod:`amqp` 1.4.1.

- ``maybe_declare`` now raises a "recoverable connection error" if
  the channel is disconnected instead of a :exc:`ChannelError` so that
  the operation can be retried.

- Redis: ``Consumer.cancel()`` is now thread safe.

    This fixes an issue when using gevent/eventlet and a
    message is handled after the consumer is canceled resulting
    in a "message for queue without consumers" error.

- Retry operations would not always respect the interval_start
  value when calculating the time to sleep for (Issue #303).

    Fix contributed by Antoine Legrand.

- Timer: Fixed "unhashable type" error on Python 3.

- Hub: Do not attempt to unregister operations on an already closed
  poller instance.

.. _version-3.0.9:

3.0.9
=====
:release-date: 2014-01-13 05:30 P.M UTC
:release-by: Ask Solem

- Now depends on :mod:`amqp` 1.4.0.

- Redis: Basic cancel for fanout based queues now sends a corresponding
  ``UNSUBSCRIBE`` command to the server.

    This fixes an issue with pidbox where reply messages could be received
    after the consumer was canceled, giving the ``"message to queue without
    consumers"`` error.

- MongoDB: Improved connection string and options handling
  (Issue #266 + Issue #120).

    Contributed by Alex Koshelev.

- SQS: Limit the number of messages when receiving in batch to 10.

    This is a hard limit enforced by Amazon so the sqs transport
    must not exceeed this value.

    Fix contributed by Eric Reynolds.

- ConsumerMixin: ``consume`` now checks heartbeat every time the
  socket times out.

    Contributed by Dustin J. Mitchell.

- Retry Policy: A max retries of 0 did not retry forever.

    Fix contributed by Antoine Legrand.

- Simple: If passing a Queue object the simple utils will now take
  default routing key from that queue.

    Contributed by Fernando Jorge Mota.

- ``repr(producer)`` no longer evaluates the underlying channnel.

- Redis: The map of Redis error classes are now exposed at the module level
  using the :func:`kombu.transport.redis.get_redis_error_classes` function.

- Async: ``Hub.close`` now sets ``.poller`` to None.

.. _version-3.0.8:

3.0.8
=====
:release-date: 2013-12-16 05:00 P.M UTC
:release-by: Ask Solem

- Serializer: loads and dumps now wraps exceptions raised into
  :exc:`~kombu.exceptions.DecodeError` and
  :exc:`kombu.exceptions.EncodeError` respectively.

    Contributed by Ionel Cristian Maries

- Redis: Would attempt to read from the wrong connection if a select/epoll/kqueue
  exception event happened.

    Fix contributed by Michael Nelson.

- Redis: Disabling ack emulation now works properly.

    Fix contributed by Michael Nelson.

- Redis: :exc:`IOError` and :exc:`OSError` are now treated as recoverable
  connection errors.

- SQS: Improved performance by reading messages in bulk.

    Contributed by Matt Wise.

- Connection Pool: Attempting to acquire from a closed pool will now
  raise :class:`RuntimeError`.

.. _version-3.0.7:

3.0.7
=====
:release-date: 2013-12-02 04:00 P.M UTC
:release-by: Ask Solem

- Fixes Python 2.6 compatibility.

- Redis: Fixes 'bad file descriptor' issue.

.. _version-3.0.6:

3.0.6
=====
:release-date: 2013-11-21 04:50 P.M UTC
:release-by: Ask Solem

- Timer: No longer attempts to hash keyword arguments (Issue #275).

- Async: Did not account for the long type for file descriptors.

    Fix contributed by Fabrice Rabaute.

- PyPy: kqueue support was broken.

- Redis: Bad pub/sub payloads no longer crashes the consumer.

- Redis: Unix socket URLs can now specify a virtual host by including
  it as a query parameter.

    Example URL specifying a virtual host using database number 3:

    .. code-block:: text

        redis+socket:///tmp/redis.sock?virtual_host=3

- ``kombu.VERSION`` is now a named tuple.

.. _version-3.0.5:

3.0.5
=====
:release-date: 2013-11-15 11:00 P.M UTC
:release-by: Ask Solem

- Now depends on :mod:`amqp` 1.3.3.

- Redis: Fixed Python 3 compatibility problem (Issue #270).

- MongoDB: Fixed problem with URL parsing when authentication used.

    Fix contributed by dongweiming.

- pyamqp: Fixed small issue when publishing the message and
  the property dictionary was set to None.

    Fix contributed by Victor Garcia.

- Fixed problem in ``repr(LaxBoundedSemaphore)``.

    Fix contributed by Antoine Legrand.

- Tests now passing on Python 3.3.

.. _version-3.0.4:

3.0.4
=====
:release-date: 2013-11-08 01:00 P.M UTC
:release-by: Ask Solem

- common.QoS: ``decrement_eventually`` now makes sure the value
  does not go below 1 if a prefetch count is enabled.

.. _version-3.0.3:

3.0.3
=====
:release-date: 2013-11-04 03:00 P.M UTC
:release-by: Ask Solem

- SQS: Properly reverted patch that caused delays between messages.

    Contributed by James Saryerwinnie

- select: Clear all registerd fds on poller.cloe

- Eventloop: unregister if EBADF raised.

.. _version-3.0.2:

3.0.2
=====
:release-date: 2013-10-29 02:00 P.M UTC
:release-by: Ask Solem

- Now depends on :mod:`amqp` version 1.3.2.

- select: Fixed problem where unregister did not properly remove
  the fd.

.. _version-3.0.1:

3.0.1
=====
:release-date: 2013-10-24 04:00 P.M UTC
:release-by: Ask Solem

- Now depends on :mod:`amqp` version 1.3.1.

- Redis: New option ``fanout_keyprefix``

    This transport option is recommended for all users as it ensures
    that broadcast (fanout) messages sent is only seen by the current
    virtual host:

    .. code-block:: python

        Connection('redis://', transport_options={'fanout_keyprefix': True})

    However, enabling this means that you cannot send or receive messages
    from older Kombu versions so make sure all of your participants
    are upgraded and have the transport option enabled.

    This will be the default behavior in Kombu 4.0.

- Distribution: Removed file ``requirements/py25.txt``.

- MongoDB: Now disables ``auto_start_request``.

- MongoDB: Enables ``use_greenlets`` if eventlet/gevent used.

- Pidbox: Fixes problem where expires header was None,
  which is a value not supported by the amq protocol.

- ConsumerMixin: New ``consumer_context`` method for starting
  the consumer without draining events.

.. _version-3.0.0:

3.0.0
=====
:release-date: 2013-10-14 04:00 P.M BST
:release-by: Ask Solem

- Now depends on :mod:`amqp` version 1.3.

- No longer supports Python 2.5

    The minimum Python version supported is now Python 2.6.0 for Python 2,
    and Python 3.3 for Python 3.

- Dual codebase supporting both Python 2 and 3.

    No longer using ``2to3``, making it easier to maintain support for
    both versions.

- pickle, yaml and msgpack deserialization is now disabled by default.

    This means that Kombu will by default refuse to handle any content type other
    than json.

    Pickle is known to be a security concern as it will happily
    load any object that is embedded in a pickle payload, and payloads
    can be crafted to do almost anything you want.  The default
    serializer in Kombu is json but it also supports a number
    of other serialization formats that it will evaluate if received:
    including pickle.

    It was always assumed that users were educated about the security
    implications of pickle, but in hindsight we don't think users
    should be expected to secure their services if we have the ability to
    be secure by default.

    By disabling any content type that the user did not explicitly
    want enabled we ensure that the user must be conscious when they
    add pickle as a serialization format to support.

    The other built-in serializers (yaml and msgpack) are also disabled
    even though they aren't considered insecure [#f1]_ at this point.
    Instead they're disabled so that if a security flaw is found in one of these
    libraries in the future, you will only be affected if you have
    explicitly enabled them.

    To have your consumer accept formats other than json you have to
    explicitly add the wanted formats to a white-list of accepted
    content types:

    .. code-block:: pycon

        >>> c = Consumer(conn, accept=['json', 'pickle', 'msgpack'])

    or when using synchronous access:

    .. code-block:: pycon

        >>> msg = queue.get(accept=['json', 'pickle', 'msgpack'])

    The ``accept`` argument was first supported for consumers in version
    2.5.10, and first supported by ``Queue.get`` in version 2.5.15
    so to stay compatible with previous versions you can enable
    the previous behavior:

        >>> from kombu import enable_insecure_serializers
        >>> enable_insecure_serializers()

    But note that this has global effect, so be very careful should you use it.

    .. rubric:: Footnotes

    .. [#f1] The PyYAML library has a :func:`yaml.load` function with some of the
             same security implications as pickle, but Kombu uses the
             :func:`yaml.safe_load` function which is not known to be affected.

- kombu.async: Experimental event loop implementation.

    This code was previously in Celery but was moved here
    to make it easier for async transport implementations.

    The API is meant to match the Tulip API which will be included
    in Python 3.4 as the ``asyncio`` module.  It's not a complete
    implementation obviously, but the goal is that it will be easy
    to change to it once that is possible.

- Utility function ``kombu.common.ipublish`` has been removed.

    Use ``Producer(..., retry=True)`` instead.

- Utility function ``kombu.common.isend_reply`` has been removed

    Use ``send_reply(..., retry=True)`` instead.

- ``kombu.common.entry_to_queue`` and ``kombu.messaging.entry_to_queue``
  has been removed.

    Use ``Queue.from_dict(name, **options)`` instead.

- Redis: Messages are now restored at the end of the list.

    Contributed by Mark Lavin.

- ``StdConnectionError`` and ``StdChannelError`` is removed
    and :exc:`amqp.ConnectionError` and :exc:`amqp.ChannelError` is used
    instead.

- Message object implementation has moved to :class:`kombu.message.Message`.

- Serailization: Renamed functions encode/decode to
  :func:`~kombu.serialization.dumps` and :func:`~kombu.serialization.loads`.

    For backward compatibility the old names are still available as aliases.

- The ``kombu.log.anon_logger`` function has been removed.

    Use :func:`~kombu.log.get_logger` instead.

- ``queue_declare`` now returns namedtuple with ``queue``, ``message_count``,
  and ``consumer_count`` fields.

- LamportClock: Can now set lock class

- :mod:`kombu.utils.clock`: Utilities for ordering events added.

- :class:`~kombu.simple.SimpleQueue` now allows you to override
  the exchange type used.

    Contributed by Vince Gonzales.

- Zookeeper transport updated to support new changes in the :mod:`kazoo`
  library.

    Contributed by Mahendra M.

- pyamqp/librabbitmq: Transport options are now forwarded as keyword arguments
    to the underlying connection (Issue #214).

- Transports may now distinguish between recoverable and irrecoverable
  connection and channel errors.

- ``kombu.utils.Finalize`` has been removed: Use
  :mod:`multiprocessing.util.Finalize` instead.

- Memory transport now supports the fanout exchange type.

    Contributed by Davanum Srinivas.

- Experimental new `Pyro`_ transport (:mod:`kombu.transport.pyro`).

    Contributed by Tommie McAfee.

.. _`Pyro`: http://pythonhosted.org/Pyro

- Experimental new `SoftLayer MQ`_ transport (:mod:`kombu.transport.SLMQ`).

    Contributed by Kevin McDonald

.. _`SoftLayer MQ`: http://www.softlayer.com/services/additional/message-queue

- Eventio: Kqueue breaks in subtle ways so select is now used instead.

- SQLAlchemy transport: Can now specify table names using the
  ``queue_tablename`` and ``message_tablename`` transport options.

    Contributed by Ryan Petrello.

Redis transport: Now supports using local UNIX sockets to communicate with the
  Redis server (Issue #1283)

    To connect using a UNIX socket you have to use the ``redis+socket``
    URL-prefix: ``redis+socket:///tmp/redis.sock``.

    This functionality was merged from the `celery-redis-unixsocket`_ project.
    Contributed by Maxime Rouyrre.

ZeroMQ transport: drain_events now supports timeout.

    Contributed by Jesper Thomschütz.

.. _`celery-redis-unixsocket`:
    https://github.com/piquadrat/celery-redis-unixsocket

.. _version-2.5.16:

2.5.16
======
:release-date: 2013-10-04 03:30 P.M BST
:release-by: Ask Solem

- Python 3: Fixed problem with dependencies not being installed.

.. _version-2.5.15:

2.5.15
======
:release-date: 2013-10-04 03:30 P.M BST
:release-by: Ask Solem

- Declaration cache: Now only keeps hash of declaration
  so that it does not keep a reference to the channel.

- Declaration cache: Now respects ``entity.can_cache_declaration``
  attribute.

- Fixes Python 2.5 compatibility.

- Fixes tests after python-msgpack changes.

- ``Queue.get``: Now supports ``accept`` argument.

.. _version-2.5.14:

2.5.14
======
:release-date: 2013-08-23 05:00 P.M BST
:release-by: Ask Solem

- safe_str did not work properly resulting in
  :exc:`UnicodeDecodeError` (Issue #248).

.. _version-2.5.13:

2.5.13
======
:release-date: 2013-08-16 04:00 P.M BST
:release-by: Ask Solem

- Now depends on :mod:`amqp` 1.0.13

- Fixed typo in Django functional tests.

- safe_str now returns Unicode in Python 2.x

    Fix contributed by Germán M. Bravo.

- amqp: Transport options are now merged with arguments
  supplied to the connection.

- Tests no longer depends on distribute, which was deprecated
  and merged back into setuptools.

    Fix contributed by Sascha Peilicke.

- ConsumerMixin now also restarts on channel related errors.

    Fix contributed by Corentin Ardeois.

.. _version-2.5.12:

2.5.12
======
:release-date: 2013-06-28 03:30 P.M BST
:release-by: Ask Solem

- Redis: Ignore errors about keys missing in the round-robin cycle.

- Fixed test suite errors on Python 3.

- Fixed msgpack test failures.

.. _version-2.5.11:

2.5.11
======
:release-date: 2013-06-25 02:30 P.M BST
:release-by: Ask Solem

- Now depends on amqp 1.0.12 (Py3 compatibility issues).

- MongoDB:  Removed cause of a "database name in URI is being ignored"
  warning.

    Fix by Flavio Percoco Premoli

- Adds ``passive`` option to :class:`~kombu.Exchange`.

    Setting this flag means that the exchange will not be declared by kombu,
    but that it must exist already (or an exception will be raised).

    Contributed by Rafal Malinowski

- Connection.info() now gives the current hostname and not the list of
  available hostnames.

    Fix contributed by John Shuping.

- pyamqp: Transport options are now forwarded as kwargs to ``amqp.Connection``.

- librabbitmq: Transport options are now forwarded as kwargs to
  ``librabbitmq.Connection``.

- librabbitmq:  Now raises :exc:`NotImplementedError` if SSL is enabled.

    The librabbitmq library does not support ssl,
    but you can use stunnel or change to the ``pyamqp://`` transport
    instead.

    Fix contributed by Dan LaMotte.

- librabbitmq: Fixed a cyclic reference at connection close.

- eventio: select implementation now removes bad file descriptors.

- eventio: Fixed Py3 compatibility problems.

- Functional tests added for py-amqp and librabbitmq transports.

- Resource.force_close_all no longer uses a mutex.

- Pidbox: Now ignores `IconsistencyError` when sending replies,
  as this error simply means that the client may no longer be alive.

- Adds new :meth:`Connection.collect <~kombu.Connection.collect>` method,
  that can be used to clean up after connections without I/O.

- ``queue_bind`` is no longer called for queues bound to
  the "default exchange" (Issue #209).

    Contributed by Jonathan Halcrow.

- The max_retries setting for retries was not respected correctly (off by one).

.. _version-2.5.10:

2.5.10
======
:release-date: 2013-04-11 06:10 P.M BST
:release-by: Ask Solem

Note about upcoming changes for Kombu 3.0
-----------------------------------------

Kombu 3 consumers will no longer accept pickle/yaml or msgpack
by default, and you will have to explicitly enable untrusted deserializers
either globally using :func:`kombu.enable_insecure_serializers`, or
using the ``accept`` argument to :class:`~kombu.Consumer`.

Changes
-------

- New utility function to disable/enable untrusted serializers.

      - :func:`kombu.disable_insecure_serializers`
      - :func:`kombu.enable_insecure_serializers`.

- Consumer: ``accept`` can now be used to specify a whitelist
  of content types to accept.

    If the accept whitelist is set and a message is received
    with a content type that is not in the whitelist then a
    :exc:`~kombu.exceptions.ContentDisallowed` exception
    is raised.  Note that this error can be handled by the already
    existing `on_decode_error` callback

    Examples:

    .. code-block:: python

        Consumer(accept=['application/json'])
        Consumer(accept=['pickle', 'json'])

- Now depends on amqp 1.0.11

- pidbox: Mailbox now supports the ``accept`` argument.

- Redis: More friendly error for when keys are missing.

- Connection URLs: The parser did not work well when there were
  multiple '+' tokens.

.. _version-2.5.9:

2.5.9
=====
:release-date: 2013-04-08 05:07 P.M BST
:release-by: Ask Solem

- Pidbox: Now warns if there are multiple nodes consuming from
  the same pidbox.

- Adds :attr:`Queue.on_declared <kombu.Queue.on_declared>`

    A callback to be called when the queue is declared,
    with signature ``(name, messages, consumers)``.

- Now uses fuzzy matching to suggest alternatives to typos in transport
  names.

- SQS: Adds new transport option ``queue_prefix``.

    Contributed by j0hnsmith.

- pyamqp: No longer overrides verify_connection.

- SQS: Now specifies the ``driver_type`` and ``driver_name``
  attributes.

    Fix contributed by Mher Movsisyan.

- Fixed bug with ``kombu.utils.retry_over_time`` when no errback
  specified.


.. _version-2.5.8:

2.5.8
=====
:release-date: 2013-03-21 04:00 P.M UTC
:release-by: Ask Solem

- Now depends on :mod:`amqp` 1.0.10 which fixes a Python 3 compatibility error.

- Redis: Fixed a possible race condition (Issue #171).

- Redis: Ack emulation/visibility_timeout can now be disabled
  using a transport option.

    Ack emulation adds quite a lot of overhead to ensure data is safe
    even in the event of an unclean shutdown.  If data loss do not worry
    you there is now an `ack_emulation` transport option you can use
    to disable it:

    .. code-block:: python

        Connection('redis://', transport_options={'ack_emulation': False})

- SQS: Fixed :mod:`boto` v2.7 compatibility (Issue #207).

- Exchange: Should not try to re-declare default exchange (``""``)
  (Issue #209).

- SQS: Long polling is now disabled by default as it was not
  implemented correctly, resulting in long delays between receiving
  messages (Issue #202).

- Fixed Python 2.6 incompatibility depending on ``exc.errno``
  being available.

    Fix contributed by Ephemera.

.. _version-2.5.7:

2.5.7
=====
:release-date: 2013-03-08 01:00 P.M UTC
:release-by: Ask Solem

- Now depends on amqp 1.0.9

- Redis: A regression in 2.5.6 caused the redis transport to
  ignore options set in ``transport_options``.

- Redis: New ``socket_timeout`` transport option.

- Redis: ``InconsistencyError`` is now regarded as a recoverable error.

- Resource pools: Will no longer attempt to release resource
  that was never acquired.

- MongoDB: Now supports the ``ssl`` option.

    Contributed by Sebastian Pawlus.

.. _version-2.5.6:

2.5.6
=====
:release-date: 2013-02-08 01:00 P.M UTC
:release-by: Ask Solem

- Now depends on amqp 1.0.8 which works around a bug found on some
  Python 2.5 installations where 2**32 overflows to 0.

.. _version-2.5.5:

2.5.5
=====
:release-date: 2013-02-07 05:00 P.M UTC
:release-by: Ask Solem

SQS: Now supports long polling (Issue #176).

    The polling interval default has been changed to 0 and a new
    transport option (``wait_time_seconds``) has been added.
    This parameter specifies how long to wait for a message from
    SQS, and defaults to 20 seconds, which is the maximum
    value currently allowed by Amazon SQS.

    Contributed by James Saryerwinnie.

- SQS: Now removes unpickleable fields before restoring messages.

- Consumer.__exit__ now ignores exceptions occurring while
  canceling the consumer.

- Virtual:  Routing keys can now consist of characters also used
  in regular expressions (e.g. parens) (Issue #194).

- Virtual: Fixed compression header when restoring messages.

    Fix contributed by Alex Koshelev.

- Virtual: ack/reject/requeue now works while using ``basic_get``.

- Virtual: Message.reject is now supported by virtual transports
  (requeue depends on individual transport support).

- Fixed typo in hack used for static analyzers.

    Fix contributed by Basil Mironenko.

.. _version-2.5.4:

2.5.4
=====
:release-date: 2012-12-10 12:35 P.M UTC
:release-by: Ask Solem

- Fixed problem with connection clone and multiple URLs (Issue #182).

    Fix contributed by Dane Guempel.

- zeromq: Now compatible with libzmq 3.2.x.

    Fix contributed by Andrey Antukh.

- Fixed Python 3 installation problem (Issue #187).

.. _version-2.5.3:

2.5.3
=====
:release-date: 2012-11-29 12:35 P.M UTC
:release-by: Ask Solem

- Pidbox: Fixed compatibility with Python 2.6

2.5.2
=====
:release-date: 2012-11-29 12:35 P.M UTC
:release-by: Ask Solem

.. _version-2.5.2:

2.5.2
=====
:release-date: 2012-11-29 12:35 P.M UTC
:release-by: Ask Solem

- [Redis] Fixed connection leak and added a new 'max_connections' transport
  option.

.. _version-2.5.1:

2.5.1
=====
:release-date: 2012-11-28 12:45 P.M UTC
:release-by: Ask Solem

- Fixed bug where return value of Queue.as_dict could not be serialized with
  JSON (Issue #177).

.. _version-2.5.0:

2.5.0
=====
:release-date: 2012-11-27 04:00 P.M UTC
:release-by: Ask Solem

- `py-amqp`_ is now the new default transport, replacing ``amqplib``.

    The new `py-amqp`_ library is a fork of amqplib started with the
    following goals:

        - Uses AMQP 0.9.1 instead of 0.8
        - Support for heartbeats (Issue #79 + Issue #131)
        - Automatically revives channels on channel errors.
        - Support for all RabbitMQ extensions
            - Consumer Cancel Notifications (Issue #131)
            - Publisher Confirms (Issue #131).
            - Exchange-to-exchange bindings: ``exchange_bind`` / ``exchange_unbind``.
        - API compatible with :mod:`librabbitmq` so that it can be used
          as a pure-python replacement in environments where rabbitmq-c cannot
          be compiled.  librabbitmq will be updated to support all the same
          features as py-amqp.

- Support for using multiple connection URL's for failover.

    The first argument to :class:`~kombu.Connection` can now be a list of
    connection URLs:

    .. code-block:: python

        Connection(['amqp://foo', 'amqp://bar'])

    or it can be a single string argument with several URLs separated by
    semicolon:

    .. code-block:: python

        Connection('amqp://foo;amqp://bar')

    There is also a new keyword argument ``failover_strategy`` that defines
    how :meth:`~kombu.Connection.ensure_connection`/
    :meth:`~kombu.Connection.ensure`/:meth:`kombu.Connection.autoretry` will
    reconnect in the event of connection failures.

    The default reconnection strategy is ``round-robin``, which will simply
    cycle through the list forever, and there's also a ``shuffle`` strategy
    that will select random hosts from the list.  Custom strategies can also
    be used, in that case the argument must be a generator yielding the URL
    to connect to.

    Example:

    .. code-block:: python

        Connection('amqp://foo;amqp://bar')

- Now supports PyDev, PyCharm, pylint and other static code analysis tools.

- :class:`~kombu.Queue` now supports multiple bindings.

    You can now have multiple bindings in the same queue by having
    the second argument be a list:

    .. code-block:: python

        from kombu import binding, Queue

        Queue('name', [
            binding(Exchange('E1'), routing_key='foo'),
            binding(Exchange('E1'), routing_key='bar'),
            binding(Exchange('E2'), routing_key='baz'),
        ])

    To enable this, helper methods have been added:

        - :meth:`~kombu.Queue.bind_to`
        - :meth:`~kombu.Queue.unbind_from`

    Contributed by Rumyana Neykova.

- Custom serializers can now be registered using Setuptools entry-points.

    See :ref:`serialization-entrypoints`.

- New :class:`kombu.common.QoS` class used as a thread-safe way to manage
  changes to a consumer or channels prefetch_count.

    This was previously an internal class used in Celery now moved to
    the :mod:`kombu.common` module.

- Consumer now supports a ``on_message`` callback that can be used to process
  raw messages (not decoded).

    Other callbacks specified using the ``callbacks`` argument, and
    the ``receive`` method will be not be called when a on message callback
    is present.

- New utility :func:`kombu.common.ignore_errors` ignores connection and
  channel errors.

    Must only be used for cleanup actions at shutdown or on connection loss.

- Support for exchange-to-exchange bindings.

    The :class:`~kombu.Exchange` entity gained ``bind_to``
    and ``unbind_from`` methods:

    .. code-block:: python

        e1 = Exchange('A')(connection)
        e2 = Exchange('B')(connection)

        e2.bind_to(e1, routing_key='rkey', arguments=None)
        e2.unbind_from(e1, routing_key='rkey', arguments=None)

    This is currently only supported by the ``pyamqp`` transport.

    Contributed by Rumyana Neykova.

.. _version-2.4.10:

2.4.10
======
:release-date: 2012-11-22 06:00 P.M UTC
:release-by: Ask Solem

- The previous versions connection pool changes broke Redis support so that
  it would always connect to localhost (default setting) no matter what
  connection parameters were provided (Issue #176).

.. _version-2.4.9:

2.4.9
=====
:release-date: 2012-11-21 03:00 P.M UTC
:release-by: Ask Solem

- Redis: Fixed race condition that could occur while trying to restore
  messages (Issue #171).

    Fix contributed by Ollie Walsh.

- Redis: Each channel is now using a specific connection pool instance,
  which is disconnected on connection failure.

- ProducerPool: Fixed possible dead-lock in the acquire method.

- ProducerPool: ``force_close_all`` no longer tries to call the non-existent
  ``Producer._close``.

- librabbitmq: Now implements ``transport.verify_connection`` so that
  connection pools will not give back connections that are no longer working.

- New and better ``repr()`` for Queue and Exchange objects.

- Python 3:  Fixed problem with running the unit test suite.

- Python 3: Fixed problem with JSON codec.

.. _version-2.4.8:

2.4.8
=====
:release-date: 2012-11-02 05:00 P.M UTC
:release-by: Ask Solem

- Redis:  Improved fair queue cycle implementation (Issue #166).

    Contributed by Kevin McCarthy.

- Redis: Unacked message restore limit is now unlimited by default.

    Also, the limit can now be configured using the ``unacked_restore_limit``
    transport option:

    .. code-block:: python

        Connection('redis://', transport_options={
            'unacked_restore_limit': 100,
        })

        A limit of 100 means that the consumer will restore at most 100
        messages at each pass.

- Redis: Now uses a mutex to ensure only one consumer restores messages at a
  time.

    The mutex expires after 5 minutes by default, but can be configured
    using the ``unacked_mutex_expire`` transport option.

- LamportClock.adjust now returns the new clock value.

- Heartbeats can now be specified in URLs.

    Fix contributed by Mher Movsisyan.

- Kombu can now be used with PyDev, PyCharm and other static analysis tools.

- Fixes problem with msgpack on Python 3 (Issue #162).

    Fix contributed by Jasper Bryant-Greene

- amqplib: Fixed bug with timeouts when SSL is used in non-blocking mode.

    Fix contributed by Mher Movsisyan


.. _version-2.4.7:

2.4.7
=====
:release-date: 2012-09-18 03:00 P.M BST
:release-by: Ask Solem

- Virtual: Unknown exchanges now default to 'direct' when sending a message.

- MongoDB: Fixed memory leak when merging keys stored in the db (Issue #159)

    Fix contributed by Michael Korbakov.

- MongoDB: Better index for MongoDB transport (Issue #158).

    This improvement will create a new compund index for queue and _id in order
    to be able to use both indexed fields for getting a new message (using
    queue field) and sorting by _id.  It'll be necessary to manually delete
    the old index from the collection.

    Improvement contributed by rmihael

.. _version-2.4.6:

2.4.6
=====
:release-date: 2012-09-12 03:00 P.M BST
:release-by: Ask Solem

- Adds additional compatibility dependencies:

    - Python <= 2.6:

        - importlib
        - ordereddict

    - Python <= 2.5

        - simplejson

.. _version-2.4.5:

2.4.5
=====
:release-date: 2012-08-30 03:36 P.M BST
:release-by: Ask Solem

- Last version broke installtion on PyPy and Jython due
  to test requirements clean-up.

.. _version-2.4.4:

2.4.4
=====
:release-date: 2012-08-29 04:00 P.M BST
:release-by: Ask Solem

- amqplib: Fixed a bug with asynchronously reading large messages.

- pyamqp: Now requires amqp 0.9.3

- Cleaned up test requirements.

.. _version-2.4.3:

2.4.3
=====
:release-date: 2012-08-25 10:30 P.M BST
:release-by: Ask Solem

- Fixed problem with amqp transport alias (Issue #154).

.. _version-2.4.2:

2.4.2
=====
:release-date: 2012-08-24 05:00 P.M BST
:release-by: Ask Solem

- Having an empty transport name broke in 2.4.1.


.. _version-2.4.1:

2.4.1
=====
:release-date: 2012-08-24 04:00 P.M BST
:release-by: Ask Solem

- Redis: Fixed race condition that could cause the consumer to crash (Issue #151)

    Often leading to the error message ``"could not convert string to float"``

- Connection retry could cause an inifite loop (Issue #145).

- The ``amqp`` alias is now resolved at runtime, so that eventlet detection
  works even if patching was done later.

.. _version-2.4.0:

2.4.0
=====
:release-date: 2012-08-17 08:00 P.M BST
:release-by: Ask Solem

- New experimental :mod:`ZeroMQ <kombu.transport.zmq` transport.

    Contributed by John Watson.

- Redis: Ack timed-out messages were not restored when using the eventloop.

- Now uses pickle protocol 2 by default to be cross-compatible with Python 3.

    The protocol can also now be changed using the :envvar:`PICKLE_PROTOCOL`
    environment variable.

- Adds ``Transport.supports_ev`` attribute.

- Pika: Queue purge was not working properly.

    Fix contributed by Steeve Morin.

- Pika backend was no longer working since Kombu 2.3

    Fix contributed by Steeve Morin.

.. _version-2.3.2:

2.3.2
=====
:release-date: 2012-08-01 06:00 P.M BST
:release-by: Ask Solem

- Fixes problem with deserialization in Python 3.

.. _version-2.3.1:

2.3.1
=====
:release-date: 2012-08-01 04:00 P.M BST
:release-by: Ask Solem

- librabbitmq: Can now handle messages that does not have a
  content_encoding/content_type set (Issue #149).

    Fix contributed by C Anthony Risinger.

- Beanstalk: Now uses localhost by default if the URL does not contain a host.

.. _version-2.3.0:

2.3.0
=====
:release-date: 2012-07-24 03:50 P.M BST
:release-by: Ask Solem

- New ``pyamqp://`` transport!

    The new `py-amqp`_ library is a fork of amqplib started with the
    following goals:

        - Uses AMQP 0.9.1 instead of 0.8
        - Should support all RabbitMQ extensions
        - API compatible with :mod:`librabbitmq` so that it can be used
          as a pure-python replacement in environments where rabbitmq-c cannot
          be compiled.

    .. _`py-amqp`: https://amqp.readthedocs.io/

    If you start using use py-amqp instead of amqplib you can enjoy many
    advantages including:

        - Heartbeat support (Issue #79 + Issue #131)
        - Consumer Cancel Notifications (Issue #131)
        - Publisher Confirms

    amqplib has not been updated in a long while, so maintaining our own fork
    ensures that we can quickly roll out new features and fixes without
    resorting to monkey patching.

    To use the py-amqp transport you must install the :mod:`amqp` library:

    .. code-block:: console

        $ pip install amqp

    and change the connection URL to use the correct transport:

    .. code-block:: pycon

        >>> conn = Connection('pyamqp://guest:guest@localhost//')


    The ``pyamqp://`` transport will be the default fallback transport
    in Kombu version 3.0, when :mod:`librabbitmq` is not installed,
    and librabbitmq will also be updated to support the same features.

- Connection now supports heartbeat argument.

    If enabled you must make sure to manually maintain heartbeats
    by calling the ``Connection.heartbeat_check`` at twice the rate
    of the specified heartbeat interval.

    E.g. if you have ``Connection(heartbeat=10)``,
    then you must call ``Connection.heartbeat_check()`` every 5 seconds.

    if the server has not sent heartbeats at a suitable rate then
    the heartbeat check method must raise an error that is listed
    in ``Connection.connection_errors``.

    The attribute ``Connection.supports_heartbeats`` has been added
    for the ability to inspect if a transport supports heartbeats
    or not.

    Calling ``heartbeat_check`` on a transport that does
    not support heartbeats results in a noop operation.

- SQS: Fixed bug with invalid characters in queue names.

    Fix contributed by Zach Smith.

- utils.reprcall: Fixed typo where kwargs argument was an empty tuple by
  default, and not an empty dict.

.. _version-2.2.6:

2.2.6
=====
:release-date: 2012-07-10 05:00 P.M BST
:release-by: Ask Solem

- Adds ``kombu.messaging.entry_to_queue`` for compat with previous versions.

.. _version-2.2.5:

2.2.5
=====
:release-date: 2012-07-10 05:00 P.M BST
:release-by: Ask Solem

- Pidbox: Now sets queue expire at 10 seconds for reply queues.

- EventIO: Now ignores ``ValueError`` raised by epoll unregister.

- MongoDB: Fixes Issue #142

    Fix by Flavio Percoco Premoli

.. _version-2.2.4:

2.2.4
=====
:release-date: 2012-07-05 04:00 P.M BST
:release-by: Ask Solem

- Support for msgpack-python 0.2.0 (Issue #143)

    The latest msgpack version no longer supports Python 2.5, so if you're
    still using that you need to depend on an earlier msgpack-python version.

    Fix contributed by Sebastian Insua

- :func:`~kombu.common.maybe_declare` no longer caches entities with the
  ``auto_delete`` flag set.

- New experimental filesystem transport.

    Contributed by Bobby Beever.

- Virtual Transports: Now support anonymous queues and exchanges.

.. _version-2.2.3:

2.2.3
=====
:release-date: 2012-06-24 05:00 P.M BST
:release-by: Ask Solem

- ``BrokerConnection`` now renamed to ``Connection``.

    The name ``Connection`` has been an alias for a very long time,
    but now the rename is official in the documentation as well.

    The Connection alias has been available since version 1.1.3,
    and ``BrokerConnection`` will still work and is not deprecated.

- ``Connection.clone()`` now works for the sqlalchemy transport.

- :func:`kombu.common.eventloop`, :func:`kombu.utils.uuid`,
  and :func:`kombu.utils.url.parse_url` can now be
  imported from the :mod:`kombu` module directly.

- Pidbox transport callback ``after_reply_message_received`` now happens
  in a finally block.

- Trying to use the ``librabbitmq://`` transport will now show the right
  name in the :exc:`ImportError` if :mod:`librabbitmq` is not installed.

    The librabbitmq falls back to the older ``pylibrabbitmq`` name for
    compatibility reasons and would therefore show ``No module named
    pylibrabbitmq`` instead of librabbitmq.


.. _version-2.2.2:

2.2.2
=====
:release-date: 2012-06-22 02:30 P.M BST
:release-by: Ask Solem

- Now depends on :mod:`anyjson` 0.3.3

- Json serializer: Now passes :class:`buffer` objects directly,
  since this is supported in the latest :mod:`anyjson` version.

- Fixes blocking epoll call if timeout was set to 0.

    Fix contributed by John Watson.

- setup.py now takes requirements from the :file:`requirements/` directory.

- The distribution directory :file:`contrib/` is now renamed to :file:`extra/`

.. _version-2.2.1:

2.2.1
=====
:release-date: 2012-06-21 01:00 P.M BST
:release-by: Ask Solem

- SQS: Default visibility timeout is now 30 minutes.

    Since we have ack emulation the visibility timeout is
    only in effect if the consumer is abrubtly terminated.

- retry argument to ``Producer.publish`` now works properly,
  when the declare argument is specified.

- Json serializer: didn't handle buffer objects (Issue #135).

    Fix contributed by Jens Hoffrichter.

- Virtual: Now supports passive argument to ``exchange_declare``.

- Exchange & Queue can now be bound to connections (which will use the default
  channel):

    .. code-block:: pycon

        >>> exchange = Exchange('name')
        >>> bound_exchange = exchange(connection)
        >>> bound_exchange.declare()

- ``SimpleQueue`` & ``SimpleBuffer`` can now be bound to connections (which
  will use the default channel).

- ``Connection.manager.get_bindings`` now works for librabbitmq and pika.

- Adds new transport info attributes:

    - ``Transport.driver_type``

        Type of underlying driver, e.g. "amqp", "redis", "sql".

    - ``Transport.driver_name``

        Name of library used e.g. "amqplib", "redis", "pymongo".

    - ``Transport.driver_version()``

        Version of underlying library.

.. _version-2.2.0:

2.2.0
=====
:release-date: 2012-06-07 03:10 P.M BST
:release-by: Ask Solem

.. _v220-important:

Important Notes
---------------

- The canonical source code repository has been moved to

    http://github.com/celery/kombu

- Pidbox: Exchanges used by pidbox are no longer auto_delete.

    Auto delete has been described as a misfeature,
    and therefore we have disabled it.

    For RabbitMQ users old exchanges used by pidbox must be removed,
    these are named ``mailbox_name.pidbox``,
    and ``reply.mailbox_name.pidbox``.

    The following command can be used to clean up these exchanges:

    .. code-block:: text

        $ VHOST=/ URL=amqp:// python -c'import sys,kombu;[kombu.Connection(
            sys.argv[-1]).channel().exchange_delete(x)
                for x in sys.argv[1:-1]]' \
            $(sudo rabbitmqctl -q list_exchanges -p "$VHOST" \
            | grep \.pidbox | awk '{print $1}') "$URL"

    The :envvar:`VHOST` variable must be set to the target RabbitMQ virtual host,
    and the :envvar:`URL` must be the AMQP URL to the server.

- The ``amqp`` transport alias will now use :mod:`librabbitmq`
  if installed.

    `py-librabbitmq`_ is a fast AMQP client for Python
    using the librabbitmq C library.

    It can be installed by:

    .. code-block:: console

        $ pip install librabbitmq

    It will not be used if the process is monkey patched by eventlet/gevent.

.. _`py-librabbitmq`: https://github.com/celery/librabbitmq

.. _v220-news:

News
----

- Redis: Ack emulation improvements.

    Reducing the possibility of data loss.

    Acks are now implemented by storing a copy of the message when the message
    is consumed.  The copy is not removed until the consumer acknowledges
    or rejects it.

    This means that unacknowledged messages will be redelivered either
    when the connection is closed, or when the visibility timeout is exceeded.

    - Visibility timeout

        This is a timeout for acks, so that if the consumer
        does not ack the message within this time limit, the message
        is redelivered to another consumer.

        The timeout is set to one hour by default, but
        can be changed by configuring a transport option:

            >>> Connection('redis://', transport_options={
            ...     'visibility_timeout': 1800,  # 30 minutes
            ... })

    **NOTE**: Messages that have not been acked will be redelivered
    if the visibility timeout is exceeded, for Celery users
    this means that ETA/countdown tasks that are scheduled to execute
    with a time that exceeds the visibility timeout will be executed
    twice (or more).  If you plan on using long ETA/countdowns you
    should tweak the visibility timeout accordingly:

    .. code-block:: python

        BROKER_TRANSPORT_OPTIONS = {'visibility_timeout': 18000}  # 5 hours

    Setting a long timeout means that it will take a long time
    for messages to be redelivered in the event of a power failure,
    but if so happens you could temporarily set the visibility timeout lower
    to flush out messages when you start up the systems again.

- Experimental `Apache ZooKeeper`_ transport

    More information is in the module reference:
    :mod:`kombu.transport.zookeeper`.

    Contributed by Mahendra M.

.. _`Apache ZooKeeper`: http://zookeeper.apache.org/

- Redis: Priority support.

    The message's ``priority`` field is now respected by the Redis
    transport by having multiple lists for each named queue.
    The queues are then consumed by in order of priority.

    The priority field is a number in the range of 0 - 9, where
    0 is the default and highest priority.

    The priority range is collapsed into four steps by default, since it is
    unlikely that nine steps will yield more benefit than using four steps.
    The number of steps can be configured by setting the ``priority_steps``
    transport option, which must be a list of numbers in **sorted order**:

    .. code-block:: pycon

        >>> x = Connection('redis://', transport_options={
        ...     'priority_steps': [0, 2, 4, 6, 8, 9],
        ... })

    Priorities implemented in this way is not as reliable as
    priorities on the server side, which is why
    nickname the feature "quasi-priorities";
    **Using routing is still the suggested way of ensuring
    quality of service**, as client implemented priorities
    fall short in a number of ways, e.g. if the worker
    is busy with long running tasks, has prefetched many messages,
    or the queues are congested.

    Still, it is possible that using priorities in combination
    with routing can be more beneficial than using routing
    or priorities alone.  Experimentation and monitoring
    should be used to prove this.

    Contributed by Germán M. Bravo.

- Redis: Now cycles queues so that consuming is fair.

    This ensures that a very busy queue won't block messages
    from other queues, and ensures that all queues have
    an equal chance of being consumed from.

    This used to be the case before, but the behavior was
    accidentally changed while switching to using blocking pop.

- Redis: Auto delete queues that are bound to fanout exchanges
  is now deleted at channel.close.

- amqplib: Refactored the drain_events implementation.

- Pidbox: Now uses ``connection.default_channel``.

- Pickle serialization: Can now decode buffer objects.

- Exchange/Queue declarations can now be cached even if
  the entity is non-durable.

    This is possible because the list of cached declarations
    are now kept with the connection, so that the entities
    will be redeclared if the connection is lost.

- Kombu source code now only uses one-level of explicit relative imports.

.. _v220-fixes:

Fixes
-----

- eventio: Now ignores ENOENT raised by ``epoll.register``, and
  EEXIST from ``epoll.unregister``.

- eventio: kqueue now ignores :exc:`KeyError` on unregister.

- Redis: ``Message.reject`` now supports the ``requeue`` argument.

- Redis: Remove superfluous pipeline call.

    Fix contributed by Thomas Johansson.

- Redis: Now sets redelivered header for redelivered messages.

- Now always makes sure references to :func:`sys.exc_info` is removed.

- Virtual: The compression header is now removed before restoring messages.

- More tests for the SQLAlchemy backend.

    Contributed by Franck Cuny.

- Url parsing did not handle MongoDB URLs properly.

    Fix contributed by Flavio Percoco Premoli.

- Beanstalk: Ignore default tube when reserving.

    Fix contributed by Zhao Xiaohong.

Nonblocking consume support
---------------------------

librabbitmq, amqplib and redis transports can now be used
non-blocking.

The interface is very manual, and only consuming messages
is non-blocking so far.

The API should not be regarded as stable or final
in any way. It is used by Celery which has very limited
needs at this point. Hopefully we can introduce a proper
callback-based API later.

- ``Transport.eventmap``

    Is a map of ``fd -> callback(fileno, event)``
    to register in an eventloop.

- ``Transport.on_poll_start()``

    Is called before every call to poll.
    The poller must support ``register(fd, callback)``
    and ``unregister(fd)`` methods.

- ``Transport.on_poll_start(poller)``

    Called when the hub is initialized.
    The poller argument must support the same
    interface as :class:`kombu.utils.eventio.poll`.

- ``Connection.ensure_connection`` now takes a callback
  argument which is called for every loop while
  the connection is down.

- Adds ``connection.drain_nowait``

    This is a non-blocking alternative to drain_events,
    but only supported by amqplib/librabbitmq.

- drain_events now sets ``connection.more_to_read`` if
  there is more data to read.

    This is to support eventloops where other things
    must be handled between draining events.

.. _version-2.1.8:

2.1.8
=====
:release-date: 2012-05-06 03:06 P.M BST
:release-by: Ask Solem

* Bound Exchange/Queue's are now pickleable.

* Consumer/Producer can now be instantiated without a channel,
  and only later bound using ``.revive(channel)``.

* ProducerPool now takes ``Producer`` argument.

* :func:`~kombu.utils.fxrange` now counts forever if the
  stop argument is set to None.
  (fxrange is like xrange but for decimals).

* Auto delete support for virtual transports were incomplete
  and could lead to problems so it was removed.

* Cached declarations (:func:`~kombu.common.maybe_declare`)
  are now bound to the underlying connection, so that
  entities are redeclared if the connection is lost.

    This also means that previously uncacheable entities
    (e.g. non-durable) can now be cached.

* compat ConsumerSet: can now specify channel.

.. _version-2.1.7:

2.1.7
=====
:release-date: 2012-04-27 06:00 P.M BST
:release-by: Ask Solem

* compat consumerset now accepts optional channel argument.

.. _version-2.1.6:

2.1.6
=====
:release-date: 2012-04-23 01:30 P.M BST
:release-by: Ask Solem

* SQLAlchemy transport was not working correctly after URL parser change.

* maybe_declare now stores cached declarations per underlying connection
  instead of globally, in the rare case that data disappears from the
  broker after connection loss.

* Django: Added South migrations.

    Contributed by Joseph Crosland.

.. _version-2.1.5:

2.1.5
=====
:release-date: 2012-04-13 03:30 P.M BST
:release-by: Ask Solem

* The url parser removed more than the first leading slash (Issue #121).

* SQLAlchemy: Can now specify url using + separator

    Example:

    .. code-block:: python

        Connection('sqla+mysql://localhost/db')

* Better support for anonymous queues (Issue #116).

    Contributed by Michael Barrett.

* ``Connection.as_uri`` now quotes url parts (Issue #117).

* Beanstalk: Can now set message TTR as a message property.

    Contributed by Andrii Kostenko

.. _version-2.1.4:

2.1.4
=====
:release-date: 2012-04-03 04:00 P.M GMT
:release-by: Ask Solem

* MongoDB:  URL parsing are now delegated to the pymongo library
  (Fixes Issue #103 and Issue #87).

    Fix contributed by Flavio Percoco Premoli and James Sullivan

* SQS:  A bug caused SimpleDB to be used even if sdb persistence
  was not enabled (Issue #108).

    Fix contributed by Anand Kumria.

* Django:  Transaction was committed in the wrong place, causing
  data cleanup to fail (Issue #115).

    Fix contributed by Daisuke Fujiwara.

* MongoDB: Now supports replica set URLs.

    Contributed by Flavio Percoco Premoli.

* Redis: Now raises a channel error if a queue key that is currently
  being consumed from disappears.

    Fix contributed by Stephan Jaekel.

* All transport 'channel_errors' lists now includes
  ``kombu.exception.StdChannelError``.

* All kombu exceptions now inherit from a common
  :exc:`~kombu.exceptions.KombuError`.

.. _version-2.1.3:

2.1.3
=====
:release-date: 2012-03-20 03:00 P.M GMT
:release-by: Ask Solem

* Fixes Jython compatibility issues.

* Fixes Python 2.5 compatibility issues.

.. _version-2.1.2:

2.1.2
=====
:release-date: 2012-03-01 01:00 P.M GMT
:release-by: Ask Solem

* amqplib: Last version broke SSL support.

.. _version-2.1.1:

2.1.1
=====
:release-date: 2012-02-24 02:00 P.M GMT
:release-by: Ask Solem

* Connection URLs now supports encoded characters.

* Fixed a case where connection pool could not recover from connection loss.

    Fix contributed by Florian Munz.

* We now patch amqplib's ``__del__`` method to skip trying to close the socket
  if it is not connected, as this resulted in an annoying warning.

* Compression can now be used with binary message payloads.

    Fix contributed by Steeve Morin.

.. _version-2.1.0:

2.1.0
=====
:release-date: 2012-02-04 10:38 P.M GMT
:release-by: Ask Solem

* MongoDB: Now supports fanout (broadcast) (Issue #98).

    Contributed by Scott Lyons.

* amqplib: Now detects broken connections by using ``MSG_PEEK``.

* pylibrabbitmq: Now supports ``basic_get`` (Issue #97).

* gevent: Now always uses the ``select`` polling backend.

* pika transport: Now works with pika 0.9.5 and 0.9.6dev.

    The old pika transport (supporting 0.5.x) is now available
    as alias ``oldpika``.

    (Note terribly latency has been experienced with the new pika
    versions, so this is still an experimental transport).

* Virtual transports: can now set polling interval via the
  transport options (Issue #96).

    Example:

    .. code-block:: pycon

        >>> Connection('sqs://', transport_options={
        ...     'polling_interval': 5.0})

    The default interval is transport specific, but usually
    1.0s (or 5.0s for the Django database transport, which
    can also be set using the ``KOMBU_POLLING_INTERVAL`` setting).

* Adds convenience function: :func:`kombu.common.eventloop`.

.. _version-2.0.0:

2.0.0
=====
:release-date: 2012-01-15 06:34 P.M GMT
:release-by: Ask Solem

.. _v200-important:

Important Notes
---------------

.. _v200-python-compatibility:

Python Compatibility
~~~~~~~~~~~~~~~~~~~~

* No longer supports Python 2.4.

    Users of Python 2.4 can still use the 1.x series.

    The 1.x series has entered bugfix-only maintenance mode, and will
    stay that way as long as there is demand, and a willingness to
    maintain it.


.. _v200-new-transports:

New Transports
~~~~~~~~~~~~~~

* ``django-kombu`` is now part of Kombu core.

    The Django message transport uses the Django ORM to store messages.

    It uses polling, with a default polling interval of 5 seconds.
    The polling interval can be increased or decreased by configuring the
    ``KOMBU_POLLING_INTERVAL`` Django setting, which is the polling
    interval in seconds as an int or a float.  Note that shorter polling
    intervals can cause extreme strain on the database: if responsiveness
    is needed you shall consider switching to a non-polling transport.

    To use it you must use transport alias ``"django"``,
    or as a URL:

    .. code-block:: text

        django://

    and then add ``kombu.transport.django`` to ``INSTALLED_APPS``, and
    run ``manage.py syncdb`` to create the necessary database tables.

    **Upgrading**

    If you have previously used ``django-kombu``, then the entry
    in ``INSTALLED_APPS`` must be changed from ``djkombu``
    to ``kombu.transport.django``:

    .. code-block:: python

        INSTALLED_APPS = (
            # …,
            'kombu.transport.django',
        )

    If you have previously used django-kombu, then there is no need
    to recreate the tables, as the old tables will be fully compatible
    with the new version.

* ``kombu-sqlalchemy`` is now part of Kombu core.

    This change requires no code changes given that the
    ``sqlalchemy`` transport alias is used.

.. _v200-news:

News
----

* :class:`kombu.mixins.ConsumerMixin` is a mixin class that lets you
  easily write consumer programs and threads.

  See :ref:`examples` and :ref:`guide-consumers`.

* SQS Transport: Added support for SQS queue prefixes (Issue #84).

    The queue prefix can be set using the transport option
    ``queue_name_prefix``:

    .. code-block:: python

        BrokerTransport('SQS://', transport_options={
            'queue_name_prefix': 'myapp'})

    Contributed by Nitzan Miron.

* ``Producer.publish`` now supports automatic retry.

    Retry is enabled by the ``reply`` argument, and retry options
    set by the ``retry_policy`` argument:

    .. code-block:: python

        exchange = Exchange('foo')
        producer.publish(message, exchange=exchange, retry=True,
                         declare=[exchange], retry_policy={
                            'interval_start': 1.0})

    See :meth:`~kombu.Connection.ensure`
    for a list of supported retry policy options.

* ``Producer.publish`` now supports a ``declare`` keyword argument.

    This is a list of entities (:class:`Exchange`, or :class:`Queue`)
    that should be declared before the message is published.

.. _v200-fixes:

Fixes
-----

* Redis transport: Timeout was multiplied by 1000 seconds when using
  ``select`` for event I/O (Issue #86).

.. _version-1.5.1:

1.5.1
=====
:release-date: 2011-11-30 01:00 P.M GMT
:release-by: Ask Solem

* Fixes issue with ``kombu.compat`` introduced in 1.5.0 (Issue #83).

* Adds the ability to disable content_types in the serializer registry.

    Any message with a content type that is disabled will be refused.
    One example would be to disable the Pickle serializer:

        >>> from kombu.serialization import registry
        # by name
        >>> registry.disable('pickle')
        # or by mime-type.
        >>> registry.disable('application/x-python-serialize')

.. _version-1.5.0:

1.5.0
=====
:release-date: 2011-11-27 06:00 P.M GMT
:release-by: Ask Solem

* kombu.pools: Fixed a bug resulting in resources not being properly released.

  This was caused by the use of ``__hash__`` to distinguish them.

* Virtual transports: Dead-letter queue is now disabled by default.

    The dead-letter queue was enabled by default to help application
    authors, but now that Kombu is stable it should be removed.
    There are after all many cases where messages should just be dropped
    when there are no queues to buffer them, and keeping them without
    supporting automatic cleanup is rather considered a resource leak
    than a feature.

    If wanted the dead-letter queue can still be enabled, by using
    the ``deadletter_queue`` transport option:

    .. code-block:: pycon

        >>> x = Connection('redis://',
        ...       transport_options={'deadletter_queue': 'ae.undeliver'})

    In addition, an :class:`UndeliverableWarning` is now emitted when
    the dead-letter queue is enabled and a message ends up there.

    Contributed by Ionel Maries Cristian.

* MongoDB transport now supports Replicasets (Issue #81).

    Contributed by Ivan Metzlar.

* The ``Connection.ensure`` methods now accepts a ``max_retries`` value
  of 0.

    A value of 0 now means *do not retry*, which is distinct from :const:`None`
    which means *retry indefinitely*.

    Contributed by Dan McGee.

* SQS Transport: Now has a lowercase ``sqs`` alias, so that it can be
  used with broker URLs (Issue #82).

    Fix contributed by Hong Minhee

* SQS Transport: Fixes KeyError on message acknowledgments (Issue #73).

    The SQS transport now uses UUID's for delivery tags, rather than
    a counter.

    Fix contributed by Brian Bernstein.

* SQS Transport: Unicode related fixes (Issue #82).

    Fix contributed by Hong Minhee.

* Redis version check could crash because of improper handling of types
  (Issue #63).

* Fixed error with `Resource.force_close_all` when resources
  were not yet properly initialized (Issue #78).

.. _version-1.4.3:

1.4.3
=====
:release-date: 2011-10-27 10:00 P.M BST
:release-by: Ask Solem

* Fixes bug in ProducerPool where too many resources would be acquired.

.. _version-1.4.2:

1.4.2
=====
:release-date: 2011-10-26 05:00 P.M BST
:release-by: Ask Solem

* Eventio: Polling should ignore `errno.EINTR`

* SQS: str.encode did only start accepting kwargs after Py2.7.

* simple_task_queue example didn't run correctly (Issue #72).

    Fix contributed by Stefan Eletzhofer.

* Empty messages would not raise an exception not able to be handled
  by `on_decode_error` (Issue #72)

    Fix contributed by Christophe Chauvet.

* CouchDB: Properly authenticate if user/password set (Issue #70)

    Fix contributed by Rafael Duran Castaneda

* Connection.Consumer had the wrong signature.

    Fix contributed by Pavel Skvazh

.. _version-1.4.1:

1.4.1
=====
:release-date: 2011-09-26 04:00 P.M BST
:release-by: Ask Solem

* 1.4.0 broke the producer pool, resulting in new connections being
  established for every acquire.


.. _version-1.4.0:

1.4.0
=====
:release-date: 2011-09-22 05:00 P.M BST
:release-by: Ask Solem

* Adds module :mod:`kombu.mixins`.

    This module contains a :class:`~kombu.mixins.ConsumerMixin` class
    that can be used to easily implement a message consumer
    thread that consumes messages from one or more
    :class:`kombu.Consumer` instances.

* New example: :ref:`task-queue-example`

    Using the ``ConsumerMixin``, default channels and
    the global connection pool to demonstrate new Kombu features.

* MongoDB transport did not work with MongoDB >= 2.0 (Issue #66)

    Fix contributed by James Turk.

* Redis-py version check did not account for beta identifiers
  in version string.

    Fix contributed by David Ziegler.

* Producer and Consumer now accepts a connection instance as the
  first argument.

    The connections default channel will then be used.

    In addition shortcut methods has been added to Connection:

    .. code-block:: pycon

        >>> connection.Producer(exchange)
        >>> connection.Consumer(queues=..., callbacks=...)

* Connection has aquired a ``connected`` attribute that
  can be used to check if the connection instance has established
  a connection.

* ``ConnectionPool.acquire_channel`` now returns the connections
  default channel rather than establising a new channel that
  must be manually handled.

* Added ``kombu.common.maybe_declare``

    ``maybe_declare(entity)`` declares an entity if it has
    not previously been declared in the same process.

* :func:`kombu.compat.entry_to_queue` has been moved to :mod:`kombu.common`

* New module :mod:`kombu.clocks` now contains an implementation
  of Lamports logical clock.

.. _version-1.3.5:

1.3.5
=====
:release-date: 2011-09-16 06:00 P.M BST
:release-by: Ask Solem

* Python 3: AMQP_PROTOCOL_HEADER must be bytes, not str.

.. _version-1.3.4:

1.3.4
=====
:release-date: 2011-09-16 06:00 P.M BST
:release-by: Ask Solem

* Fixes syntax error in pools.reset


.. _version-1.3.3:

1.3.3
=====
:release-date: 2011-09-15 02:00 P.M BST
:release-by: Ask Solem

* pools.reset did not support after forker arguments.

.. _version-1.3.2:

1.3.2
=====
:release-date: 2011-09-10 01:00 P.M BST
:release-by: Mher Movsisyan

* Broke Python 2.5 compatibility by importing ``parse_qsl`` from ``urlparse``

* Connection.default_channel is now closed when connection is revived
  after connection failures.

* Pika: Channel now supports the ``connection.client`` attribute
  as required by the simple interface.

* pools.set_limit now raises an exception if the limit is lower
  than the previous limit.

* pools.set_limit no longer resets the pools.

.. _version-1.3.1:

1.3.1
=====
:release-date: 2011-10-07 03:00 P.M BST
:release-by: Ask Solem

* Last release broke after fork for pool reinitialization.

* Producer/Consumer now has a ``connection`` attribute,
  giving access to the :class:`Connection` of the
  instance.

* Pika: Channels now have access to the underlying
  :class:`Connection` instance using ``channel.connection.client``.

    This was previously required by the ``Simple`` classes and is now
    also required by :class:`Consumer` and :class:`Producer`.

* Connection.default_channel is now closed at object revival.

* Adds kombu.clocks.LamportClock.

* compat.entry_to_queue has been moved to new module :mod:`kombu.common`.

.. _version-1.3.0:

1.3.0
=====
:release-date: 2011-10-05 01:00 P.M BST
:release-by: Ask Solem

* Broker connection info can be now be specified using URLs

    The broker hostname can now be given as a URL instead, of the format:

    .. code-block:: text

        transport://user:password@hostname:port/virtual_host

    for example the default broker is expressed as:

    .. code-block:: pycon

        >>> Connection('amqp://guest:guest@localhost:5672//')

    Transport defaults to amqp, and is not required.
    user, password, port and virtual_host is also not mandatory and
    will default to the corresponding transports default.

    .. note::

        Note that the path component (virtual_host) always starts with a
        forward-slash.  This is necessary to distinguish between the virtual
        host '' (empty) and '/', which are both acceptable virtual host names.

        A virtual host of '/' becomes::

        .. code-block:: text

            amqp://guest:guest@localhost:5672//

        and a virtual host of '' (empty) becomes:

        .. code-block:: text

            amqp://guest:guest@localhost:5672/

        So the leading slash in the path component is **always required**.

* Now comes with default global connection and producer pools.

    The acquire a connection using the connection parameters
    from a :class:`Connection`:

    .. code-block:: pycon

        >>> from kombu import Connection, connections
        >>> connection = Connection('amqp://guest:guest@localhost//')
        >>> with connections[connection].acquire(block=True):
        ...     # do something with connection

    To acquire a producer using the connection parameters
    from a :class:`Connection`:

    .. code-block:: pycon

        >>> from kombu import Connection, producers
        >>> connection = Connection('amqp://guest:guest@localhost//')
        >>> with producers[connection].acquire(block=True):
        ...     producer.publish({'hello': 'world'}, exchange='hello')

    Acquiring a producer will in turn also acquire a connection
    from the associated pool in ``connections``, so you the number
    of producers is bound the same limit as number of connections.

    The default limit of 100 connections per connection instance
    can be changed by doing:

    .. code-block:: pycon

        >>> from kombu import pools
        >>> pools.set_limit(10)

    The pool can also be forcefully closed by doing:

    .. code-block:: pycon

        >>> from kombu import pools
        >>> pool.reset()

* SQS Transport: Persistence using SimpleDB is now disabled by default,
  after reports of unstable SimpleDB connections leading to errors.

* :class:`Producer` can now be used as a context manager.

* ``Producer.__exit__`` now properly calls ``release`` instead of close.

    The previous behavior would lead to a memory leak when using
    the :class:`kombu.pools.ProducerPool`

* Now silences all exceptions from `import ctypes` to match behaviour
  of the standard Python uuid module, and avoid passing on MemoryError
  exceptions on SELinux-enabled systems (Issue #52 + Issue #53)

* ``amqp`` is now an alias to the ``amqplib`` transport.

* ``kombu.syn.detect_environment`` now returns 'default', 'eventlet', or
  'gevent' depending on what monkey patches have been installed.

* Serialization registry has new attribute ``type_to_name`` so it is
  possible to lookup serializater name by content type.

* Exchange argument to ``Producer.publish`` can now be an :class:`Exchange`
  instance.

* ``compat.Publisher`` now supports the ``channel`` keyword argument.

* Acking a message on some transports could lead to :exc:`KeyError` being
  raised (Issue #57).

* Connection pool:  Connections are no long instantiated when the pool is
  created, but instantiated as needed instead.

* Tests now pass on PyPy.

* ``Connection.as_uri`` now includes the password if the keyword argument
  ``include_password`` is set.

* Virtual transports now comes with a default ``default_connection_params``
  attribute.

.. _version-1.2.1:

1.2.1
=====
:release-date: 2011-07-29 12:52 P.M BST
:release-by: Ask Solem

* Now depends on amqplib >= 1.0.0.

* Redis: Now automatically deletes auto_delete queues at ``basic_cancel``.

* ``serialization.unregister`` added so it is possible to remove unwanted
  seralizers.

* Fixes MemoryError while importing ctypes on SELinux (Issue #52).

* ``Connection.autoretry`` is a version of ``ensure`` that works
  with arbitrary functions (i.e. it does not need an associated object
  that implements the ``revive`` method.

  Example usage:

  .. code-block:: python

        channel = connection.channel()
        try:
            ret, channel = connection.autoretry(send_messages, channel=channel)
        finally:
            channel.close()

* ``ConnectionPool.acquire`` no longer force establishes the connection.

    The connection will be established as needed.

* ``Connection.ensure`` now supports an ``on_revive`` callback
  that is applied whenever the connection is re-established.

* ``Consumer.consuming_from(queue)`` returns True if the Consumer is
  consuming from ``queue``.

* ``Consumer.cancel_by_queue`` did not remove the queue from ``queues``.

* ``compat.ConsumerSet.add_queue_from_dict`` now automatically declared
  the queue if ``auto_declare`` set.

.. _version-1.2.0:

1.2.0
=====
:release-date: 2011-07-15 12:00 P.M BST
:release-by: Ask Solem

* Virtual: Fixes cyclic reference in Channel.close (Issue #49).

* Producer.publish: Can now set additional properties using keyword
  arguments (Issue #48).

* Adds Queue.no_ack option to control the no_ack option for individual queues.

* Recent versions broke pylibrabbitmq support.

* SimpleQueue and SimpleBuffer can now be used as contexts.

* Test requirements specifies PyYAML==3.09 as 3.10 dropped Python 2.4 support

* Now properly reports default values in Connection.info/.as_uri

.. _version-1.1.6:

1.1.6
=====
:release-date: 2011-06-13 04:00 P.M BST
:release-by: Ask Solem

* Redis: Fixes issue introduced in 1.1.4, where a redis connection
  failure could leave consumer hanging forever.

* SQS: Now supports fanout messaging by using SimpleDB to store routing
  tables.

    This can be disabled by setting the `supports_fanout` transport option:

        >>> Connection(transport='SQS',
        ...            transport_options={'supports_fanout': False})

* SQS: Now properly deletes a message when a message is acked.

* SQS: Can now set the Amazon AWS region, by using the ``region``
  transport option.

* amqplib: Now uses `localhost` as default hostname instead of raising an
  error.

.. _version-1.1.5:

1.1.5
=====
:release-date: 2011-06-07 06:00 P.M BST
:release-by: Ask Solem

* Fixes compatibility with redis-py 2.4.4.

.. _version-1.1.4:

1.1.4
=====
:release-date: 2011-06-07 04:00 P.M BST
:release-by: Ask Solem

* Redis transport: Now requires redis-py version 2.4.4 or later.

* New Amazon SQS transport added.

    Usage:

        >>> conn = Connection(transport='SQS',
        ...                   userid=aws_access_key_id,
        ...                   password=aws_secret_access_key)

    The environment variables :envvar:`AWS_ACCESS_KEY_ID` and
    :envvar:`AWS_SECRET_ACCESS_KEY` are also supported.

* librabbitmq transport: Fixes default credentials support.

* amqplib transport: Now supports `login_method` for SSL auth.

    :class:`Connection` now supports the `login_method`
    keyword argument.

    Default `login_method` is ``AMQPLAIN``.

.. _version-1.1.3:

1.1.3
=====
:release-date: 2011-04-21 04:00 P.M CEST
:release-by: Ask Solem

* Redis: Consuming from multiple connections now works with Eventlet.

* Redis: Can now perform channel operations while the channel is in
  BRPOP/LISTEN mode (Issue #35).

    Also the async BRPOP now times out after 1 second, this means that
    canceling consuming from a queue/starting consuming from additional queues
    has a latency of up to one second (BRPOP does not support subsecond
    timeouts).

* Virtual: Allow channel objects to be closed multiple times without error.

* amqplib: ``AttributeError`` has been added to the list of known
  connection related errors (:attr:`Connection.connection_errors`).

* amqplib: Now converts :exc:`SSLError` timeout errors to
  :exc:`socket.timeout` (http://bugs.python.org/issue10272)

* Ensures cyclic references are destroyed when the connection is closed.

.. _version-1.1.2:

1.1.2
=====
:release-date: 2011-04-06 04:00 P.M CEST
:release-by: Ask Solem

* Redis: Fixes serious issue where messages could be lost.

    The issue could happen if the message exceeded a certain number
    of kilobytes in size.

    It is recommended that all users of the Redis transport should
    upgrade to this version, even if not currently experiencing any
    issues.

.. _version-1.1.1:

1.1.1
=====
:release-date: 2011-04-05 03:51 P.M CEST
:release-by: Ask Solem

* 1.1.0 started using ``Queue.LifoQueue`` which is only available
  in Python 2.6+ (Issue #33).  We now ship with our own LifoQueue.


.. _version-1.1.0:

1.1.0
=====
:release-date: 2011-04-05 01:05 P.M CEST
:release-by: Ask Solem

.. _v110-important:

Important Notes
---------------

* Virtual transports: Message body is now base64 encoded by default
  (Issue #27).

    This should solve problems sending binary data with virtual
    transports.

    Message compatibility is handled by adding a ``body_encoding``
    property, so messages sent by older versions is compatible
    with this release.  However -- If you are accessing the messages
    directly not using Kombu, then you have to respect
    the ``body_encoding`` property.

    If you need to disable base64 encoding then you can do so
    via the transport options:

    .. code-block:: python

        Connection(transport='...',
                   transport_options={'body_encoding': None})

    **For transport authors**:

        You don't have to change anything in your custom transports,
        as this is handled automatically by the base class.

        If you want to use a different encoder you can do so by adding
        a key to ``Channel.codecs``.  Default encoding is specified
        by the ``Channel.body_encoding`` attribute.

        A new codec must provide two methods: ``encode(data)`` and
        ``decode(data)``.

* ConnectionPool/ChannelPool/Resource: Setting ``limit=None`` (or 0)
  now disables pool semantics, and will establish and close
  the resource whenever acquired or released.

* ConnectionPool/ChannelPool/Resource: Is now using a LIFO queue
  instead of the previous FIFO behavior.

    This means that the last resource released will be the one
    acquired next.  I.e. if only a single thread is using the pool
    this means only a single connection will ever be used.

* Connection: Cloned connections did not inherit transport_options
  (``__copy__``).

* contrib/requirements is now located in the top directory
  of the distribution.

* MongoDB: Now supports authentication using the ``userid`` and ``password``
  arguments to :class:`Connection` (Issue #30).

* Connection: Default autentication credentials are now delegated to
  the individual transports.

    This means that the ``userid`` and ``password`` arguments to
    Connection is no longer *guest/guest* by default.

    The amqplib and pika transports will still have the default
    credentials.

* :meth:`Consumer.__exit__` did not have the correct signature (Issue #32).

* Channel objects now have a ``channel_id`` attribute.

* MongoDB: Version sniffing broke with development versions of
    mongod (Issue #29).

* New environment variable :envvar:`KOMBU_LOG_CONNECTION` will now emit debug
    log messages for connection related actions.

  :envvar:`KOMBU_LOG_DEBUG` will also enable :envvar:`KOMBU_LOG_CONNECTION`.

.. _version-1.0.7:

1.0.7
=====
:release-date: 2011-03-28 05:45 P.M CEST
:release-by: Ask Solem

* Now depends on anyjson 0.3.1

    cjson is no longer a recommended json implementation, and anyjson
    will now emit a deprecation warning if used.

* Please note that the Pika backend only works with version 0.5.2.

    The latest version (0.9.x) drastically changed API, and it is not
    compatible yet.

* on_decode_error is now called for exceptions in message_to_python
  (Issue #24).

* Redis: did not respect QoS settings.

* Redis: Creating a connection now ensures the connection is established.

    This means ``Connection.ensure_connection`` works properly with
    Redis.

* consumer_tag argument to ``Queue.consume`` can't be :const:`None`
  (Issue #21).

    A None value is now automatically converted to empty string.
    An empty string will make the server generate a unique tag.

* Connection now supports a ``transport_options`` argument.

    This can be used to pass additional arguments to transports.

* Pika: ``drain_events`` raised :exc:`socket.timeout` even if no timeout
  set (Issue #8).

.. version-1.0.6:

1.0.6
=====
:release-date: 2011-03-22 04:00 P.M CET
:release-by: Ask Solem

* The ``delivery_mode`` aliases (persistent/transient) were not automatically
  converted to integer, and would cause a crash if using the amqplib
  transport.

* Redis: The redis-py :exc:`InvalidData` exception suddenly changed name to
  :exc:`DataError`.

* The :envvar:`KOMBU_LOG_DEBUG` environment variable can now be set to log all
  channel method calls.

  Support for the following environment variables have been added:

    * :envvar:`KOMBU_LOG_CHANNEL` will wrap channels in an object that
      logs every method call.

    * :envvar:`KOMBU_LOG_DEBUG` both enables channel logging and configures the
      root logger to emit messages to standard error.

    **Example Usage**:

    .. code-block:: console

        $ KOMBU_LOG_DEBUG=1 python
        >>> from kombu import Connection
        >>> conn = Connection()
        >>> channel = conn.channel()
        Start from server, version: 8.0, properties:
            {u'product': 'RabbitMQ',..............  }
        Open OK! known_hosts []
        using channel_id: 1
        Channel open
        >>> channel.queue_declare('myq', passive=True)
        [Kombu channel:1] queue_declare('myq', passive=True)
        (u'myq', 0, 1)

.. _version-1.0.5:

1.0.5
=====
:release-date: 2011-03-17 04:00 P.M CET
:release-by: Ask Solem

* Fixed memory leak when creating virtual channels.  All virtual transports
  affected (redis, mongodb, memory, django, sqlalchemy, couchdb, beanstalk).

* Virtual Transports: Fixed potential race condition when acking messages.

    If you have been affected by this, the error would show itself as an
    exception raised by the OrderedDict implementation. (``object no longer
    exists``).

* MongoDB transport requires the ``findandmodify`` command only available in
  MongoDB 1.3+, so now raises an exception if connected to an incompatible
  server version.

* Virtual Transports: ``basic.cancel`` should not try to remove unknown
  consumer tag.

.. _version-1.0.4:

1.0.4
=====
:release-date: 2011-02-28 04:00 P.M CET
:release-by: Ask Solem

* Added Transport.polling_interval

    Used by django-kombu to increase the time to sleep between SELECTs when
    there are no messages in the queue.

    Users of django-kombu should upgrade to django-kombu v0.9.2.

.. _version-1.0.3:

1.0.3
=====
:release-date: 2011-02-12 04:00 P.M CET
:release-by: Ask Solem

* ConnectionPool: Re-connect if amqplib connection closed

* Adds ``Queue.as_dict`` + ``Exchange.as_dict``.

* Copyright headers updated to include 2011.

.. _version-1.0.2:

1.0.2
=====
:release-date: 2011-01-31 10:45 P.M CET
:release-by: Ask Solem

* amqplib: Message properties were not set properly.
* Ghettoq backend names are now automatically translated to the new names.

.. _version-1.0.1:

1.0.1
=====
:release-date: 2011-01-28 12:00 P.M CET
:release-by: Ask Solem

* Redis: Now works with Linux (epoll)

.. _version-1.0.0:

1.0.0
=====
:release-date: 2011-01-27 12:00 P.M CET
:release-by: Ask Solem

* Initial release

.. _version-0.1.0:

0.1.0
=====
:release-date: 2010-07-22 04:20 P.M CET
:release-by: Ask Solem

* Initial fork of carrot