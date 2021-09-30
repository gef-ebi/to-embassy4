.. include:: global.rst

.. figure:: _static/img/atl.site.logo.png
   :align: left

|EBI| Cloud Portal and BioExcel Portal To Embassy4 migration
============================================================

:Created: |today|

|hr|

Summary
=======

As explained in the |EMBL|-EBI |VAC| team's `Documentation <https://docs.embassy.ebi.ac.uk/index.html>`_,
there is a new version of the EBI's OpenStack cloud software already in operation called
(internally) "Embassy Cloud Version 4", or `Embassy4 <https://uk1.embassy.ebi.ac.uk>`_.

Of the previous version of Embassy ("Embassy Cloud Version 3"), one part -- `extcloud06` -- has
already been decommissioned, and following internal tests this appears to have completed
successfully
[#f1]_.

On **Monday Nov. 1st 2021** the second and final part -- `extcloud05 <https://extcloud05.ebi.ac.uk/>`_
-- will be decommissioned. **We therefore we need to have all remaining activities which use
extcloud05 migrated to the new Embassy4 by Friday Oct. 29th 2021**.

Impact
======

The following EBI-managed services (i.e. the |UI|\ s and the |REST| services) have both already
migrated to `Embassy4` [#f2]_, however currently the |VM|\ s which they create/"spin up" are still being
deployed to the legacy `extcloud05`.

 * |ECP| (https://cloud-portal.ebi.ac.uk)
 * BioExcel Cloud Portal (https://bioexcel.ebi.ac.uk)

.. warning:: Our primary concern at this stage is that both ECP and BioExcel portal application
   developers will need to make changes to their applications/VMs by October 29th so that the
   applications/VMs can be deployed to `Embassy4`. If it is not possible to do so before that
   date then the applications/VMs will no longer run.

We are currently assessing the impact of a migration to `Embassy4` on our ``cpa-bioexcel-cwl``
(https://github.com/EMBL-EBI-TSI/cpa-bioexcel-cwl) Cloud Portal BioExcel application/VM, and so
far the principle changes to make it compatible with `Embassy4` are as follows :

 * The use of username/password combination credentials is not possible in `Embassy4`, instead
   "Applicaton Credentials" [#f3]_ are now used and passed in environment variables (e.g.
   ``OS_APPLICATION_CREDENTIAL_SECRET`` and ``OS_APPLICATION_CREDENTIAL_ID``).
 * The version of Terraform which the portals use for deployment purposes needed to be changed, and
   as a result the :file:`deploy.sh`, :file:`state.sh` and :file:`destroy.sh` scripts used to control the
   application/VM lifecycle (e.g. https://github.com/EMBL-EBI-TSI/cpa-bioexcel-cwl/tree/master/ostack)
   also need to be changed.

Action Plan
===========

Provisionally we have set ourselves the following timelines over the coming weeks :

 * Week 1 (Sep. 27 - Oct 1)
    * Launch this website
    * Reach out to ECP/BioExcel portal users to explain what's happening
    * Continue application/VM change requirement investigations
 * Week 2 (Oct 4 - Oct 8)
    * Finalise application/VM change requirement investigations
    * Update this website as new details arrive
    * Switch our ECP "development" environment [#f4]_ to deploy applications/VMs to `Embassy4`
 * Week 3 (Oct 11 - Oct 15)
    * Summary update via email - invite users to use our ECP "development" environment
    * Monitor ECP "development" environments
    * Continue to update this website as new details arrive
 * Week 4 (Oct 18 - Oct 22)
    * Switch our ECP and BioExcel "production" environment to deploy applications/VMs to `Embassy4`
    * Summary update via email - invite people to use our ECP and BioExcel "production" environment
 * Week 5 (Oct 25 - Oct 29)
    * Monitor ECP and BioExcel "production" environments
 * Week 6 onwards (Nov 1 onwards)
    * `extcloud05` decommissioned

Throughout this process you are welcome to contact Geoff (gef@ebi.ac.uk) and/or Alberto
(albeus@ebi.ac.uk) with any queries or requests for assistance and we will try our best
to help however we can.

.. toctree::
   :maxdepth: 2
   :caption: Contents:

|hr|

.. rubric:: Footnotes

.. [#f1] `Embassy Cloud Version 3`'s `extcloud06 <https://extcloud06.ebi.ac.uk>`_ has already been
         decommissioned, and although this impacted the portals, the change did not affect portal
         users.
.. [#f2] These services were on `extcloud06`, but are now served from `Embassy4` and being tested.
.. [#f3] Information such as https://access.redhat.com/documentation/en-us/red_hat_openstack_platform/14/html/users_and_identity_management_guide/application_credentials
         may be useful for guidance.
.. [#f4] Unfortunately we do not have a "development" environment for the BioExcel portal, only ECP
         at https://dev.portal.tsi.ebi.ac.uk/.
