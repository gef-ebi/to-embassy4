.. include:: global.rst

.. figure:: _static/img/atl.site.logo.png
   :align: left

BioExcel Portal and |EBI| Cloud Portal (|ECP|) To Embassy4 migration
====================================================================

:Created: |today|

|hr|

.. _summary:

Summary
=======

As explained in the |EMBL|-EBI |VAC| team's `Documentation <https://docs.embassy.ebi.ac.uk/index.html>`_
[#f1]_, there is a new version of the EBI's OpenStack cloud software already in operation called
(internally) "Embassy Cloud Version 4", or `Embassy4 <https://uk1.embassy.ebi.ac.uk>`_.

Of the previous version of Embassy ("Embassy Cloud Version 3"), one part -- `extcloud06` -- has
already been decommissioned, and following internal tests this appears to have completed
successfully
[#f2]_.

On **Monday Nov. 1st 2021** the second and final part -- `extcloud05 <https://extcloud05.ebi.ac.uk/>`_
-- will be decommissioned. **We therefore we need to have all remaining activities which use
extcloud05 migrated to the new Embassy4 by Friday Oct. 29th 2021**.

Update: Oct 27th 2021
=====================

We have finished testing `CWL Training <https://github.com/EMBL-EBI-TSI/cpa-bioexcel-cwl>`_ and
it is deploying successfully to Embassy4 (albeit with provisos - see next paragraph) from either
of the following :

 #. https://bioexcel.ebi.ac.uk/
 #. https://cloud-portal.ebi.ac.uk/

.. warning:: As mentioned in the previous update of Oct 20th below, it's very unlikely any other
             applications will deploy successfully - this is primarily because we use Terraform
             ver. 1.0.4, whereas it's expected that all unmodified applications will be using
             deploy/destroy scripts with incompatible Terraform ver. 0.9.1 or earlier syntax.

Provisos
-------- 

 #. We haven't yet obtained sufficient resources on the Embassy4 platform to deploy anything more
    than a single instance and destroy it immediately after use.
 #. Due to the recent change to the EBI mailserver (e.g. using |2FA|, restricted network access) the
    portal applications cannot send advisory emails to portal users (e.g. on destroying a deployed
    instance, or user request to join a team). |br| 
    We are currently in discussions to access alternative facilities but this process is ongoing -
    in the meantime the portals will display error messages when they find themselves unable to
    send emails.
 #. The "Cloud resources" data displayed on BioExcel portal deployment is unhelpful, i.e. all values
    are 0 (zero).

Update: Oct 20th 2021
=====================

We have today made changes to our production server such that it now uses a newer version of
Terraform (ver. 1.0.4) and no longer accepts username/password credentials (further details
available in the :ref:`technical` section).

Using the `CWL Training <https://github.com/EMBL-EBI-TSI/cpa-bioexcel-cwl>`_ application as an
exemplar migration project, this is has been migrated to Embassy4 and a new version "2.0" is
currently being tested in https://bioexcel.ebi.ac.uk/. For `ECP <https://cloud-portal.ebi.ac.uk/>`_
users, the legacy shared "CWL VM environment" is still visible but will no longer work and will in
due course be replaced.

.. _impact:

Impact
======

Of the following EBI-managed services, the |UI|\ s and |REST| services have both already migrated
to `Embassy4` [#f3]_, however currently the |VM|\ s which they create/"spin up" are still being
deployed to the legacy `extcloud05` :

 * |ECP| (https://cloud-portal.ebi.ac.uk)
 * BioExcel Cloud Portal (https://bioexcel.ebi.ac.uk)

.. warning:: Our primary concern at this stage is that both ECP and BioExcel portal application
   developers will need to make changes to their applications/VMs by October 29th so that the
   applications/VMs can be deployed to `Embassy4`. If it is not possible to do so before that
   date then the applications/VMs will no longer run from Monday Nov. 1st.

.. warning:: The anticipated "At Risk" period for both the ECP and the BioExcel portals is
   during the period Mon. 18th, Tue. 19th and Wed. 20th October. During this time either or
   both portals may be offline for a number of hours, although of course we will aim to keep
   any disruption to the minimum necessary.

We are currently assessing the impact of a migration to `Embassy4` on our ``cpa-bioexcel-cwl``
(https://github.com/EMBL-EBI-TSI/cpa-bioexcel-cwl) Cloud Portal BioExcel application/VM, and so
far the principle changes to make it compatible with `Embassy4` are as follows :

 * The use of username/password combination credentials is not possible in `Embassy4`, instead
   "Applicaton Credentials" [#f4]_ are now used and passed in environment variables (e.g.
   ``OS_APPLICATION_CREDENTIAL_SECRET`` and ``OS_APPLICATION_CREDENTIAL_ID``).
 * The version of Terraform which the portals use for deployment purposes needed to be changed, and
   as a result the :file:`deploy.sh`, :file:`state.sh` and :file:`destroy.sh` scripts used to control the
   application/VM lifecycle (e.g. https://github.com/EMBL-EBI-TSI/cpa-bioexcel-cwl/tree/master/ostack)
   also need to be changed.

More details of these changes appear in the :ref:`technical` section.

Action Plan
===========

Provisionally we have set ourselves the following timelines over the coming weeks :

 * Week 1 (Oct 11 - Oct 15)
    * Launch this website
    * Reach out to ECP/BioExcel portal users to explain what's happening, and to invite ECP
      users to use our "development" environment [#f5]_.
    * Continue to update this website as new details arrive
 * Week 2 (Oct 18 - Oct 22)
    * Switch our ECP and BioExcel "production" environment to deploy applications/VMs to `Embassy4`
    * Summary update via email - invite people to use our ECP and BioExcel "production" environment
 * Week 3 (Oct 25 - Oct 29)
    * Monitor ECP and BioExcel "production" environments
 * Week 4 onwards (Nov 1 onwards)
    * `extcloud05` decommissioned

Throughout this process you are welcome to contact the BioExcel/|ECP| issue tracking email address
-- ecp@ebi.ac.uk -- with any queries or requests for assistance and we will try our best to help
however we can. There's also a minimal :ref:`faq` which may help.

Further Information
-------------------

.. toctree::
   :maxdepth: 2

   technical/index

.. toctree::
   :maxdepth: 2

   FAQ/index

.. toctree::
   :maxdepth: 2

   contact/index

|hr|

.. rubric:: Footnotes

.. [#f1] At the time of writing their SSL certificate has expired and needs replacing, so there
         may be a warning when you attempt to visit their website.
.. [#f2] `Embassy Cloud Version 3`'s `extcloud06 <https://extcloud06.ebi.ac.uk>`_ has already been
         decommissioned, and although this impacted the portals, the change did not affect portal
         users.
.. [#f3] These services were on `extcloud06`, but are now served from `Embassy4` and being tested.
.. [#f4] Information such as RedHat's `application_credentials <https://access.redhat.com/documentation/en-us/red_hat_openstack_platform/14/html/users_and_identity_management_guide/application_credentials>`_
         and `openstack client <https://access.redhat.com/documentation/en-us/red_hat_openstack_platform/13/html/command_line_interface_reference/the_openstack_client>`_
         documentation may be useful for guidance.
.. [#f5] Unfortunately we do not have a "development" environment for the BioExcel portal, only ECP
         at https://dev.portal.tsi.ebi.ac.uk/.
