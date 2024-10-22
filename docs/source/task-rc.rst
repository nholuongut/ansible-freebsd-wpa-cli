rc.yml
======

Synopsis
--------

Configuration of /etc/rc.d

<TODO>

.. seealso::
   * Annotated Source code :ref:`as_rc.yml`
   * See `Practical rc.d scripting in BSD <https://www.freebsd.org/doc/en/articles/rc-scripting/index.html>`_


rc.conf.patch
-------------

.. literalinclude:: ../../files/rc.conf.patch
   :language: sh
   :emphasize-lines: 7-9
   :linenos:


network.subr.patch
------------------

.. literalinclude:: ../../files/network.subr.patch
   :language: sh
   :emphasize-lines: 7,15
   :linenos:
