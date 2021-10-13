.. include:: ../global.rst

.. _faq:

|FAQ|\ s
========

Does this migration affect me directly?
---------------------------------------

Only if you:

 #. intend to make further use of the BioExcel/ECP portals; and/or
 #. you intend to actively maintain an application used in the portals.

Ultimately, if you are a portal user, if the application you intend to use does not get updated by
the application developer(s) for deployment to the Embassy4 environment, then unfortunately it will
not be possible to launch that application.

We are :ref:`keen to hear from you <contact>` if you have previously developed an application used
by the portal (or managed a portal team) but you are no longer in a position to maintain it.

Does any of this affect BioBB?
------------------------------

We currently do not believe that there will be any impact on BioBB but we will be testing this
in due course.

How do I obtain a pair of ID/Secret credentials?
------------------------------------------------

Unless you have direct access to Embassy4 you will need to be assigned an ID and Secret for your
application by someone with such access. We are currently investigating the best way to create
such credentials, but for testing purposes please :ref:`contact us <contact>`.

What about the BioExcel portal client (bioexcel-cli)?
-----------------------------------------------------

We are aware of the presence of the "Bioexcel portal client - e2e testing tool"
(https://github.com/EMBL-EBI-TSI/bioexcel-cli) which has been used to launch the following
applications :

 * PyMDSetup
 * Chromatin Dynamics
 * COMPSs
 * NAFlex
 * PyMDSetup
 * Chromatin Dynamics
 * CWL VM environment
 * Jupyter Server Instance

We are currently investigating the impact of this migration on this application.
