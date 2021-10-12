.. include:: ../global.rst

.. _technical:

Technical
=========

For general information, the notes below are based on the modifications required to make the
https://github.com/EMBL-EBI-TSI/cpa-bioexcel-cwl ECP application runnable on the new Embassy4.

Credentials
-----------

As mentioned in the :ref:`impact` statement; the credential submission mechanism has changed, and
now instead of using a username/password mechanism, Embassy4 expects to find the following
environment variables defined during deployment :

 +--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
 | Environment Variable                 | Description (derived from `RedHat docs <https://access.redhat.com/documentation/en-us/red_hat_openstack_platform/13/html/command_line_interface_reference/the_openstack_client>`_) |
 +======================================+====================================================================================================================================================================================+
 | ``OS_APPLICATION_CREDENTIAL_ID``     | With v3applicationcredential: application credential ID                                                                                                                            |
 +--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
 | ``OS_APPLICATION_CREDENTIAL_SECRET`` |With v3applicationcredential: application credential auth secret                                                                                                                    |
 +--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Other Changes
-------------

**Please visit** the corresponding 
`github.com compare <https://github.com/EMBL-EBI-TSI/cpa-bioexcel-cwl/compare/1dc1e987317eb313fa0a1f84b6548cafeb75d304...HEAD>`_
to see the changes made to the application, particularly the changes marked "mandatory" in the diagram below,
e.g. :file:`deploy.sh`, :file:`destroy.sh`, :file:`instance.tf`.

**Note :** It was discovered during the update process that the application had a "pip installer issue" which
was not related to the migration but was instead a python versioning issue. See 
`github.com <https://github.com/EMBL-EBI-TSI/cpa-bioexcel-cwl/commit/5632d7a02722d9a6bd2e2c0781c27f6fe88776bf>`_ and
`stackoverflow.com <https://stackoverflow.com/questions/65122957/resolving-new-pip-backtracking-runtime-issue>`_ for
further details if interested.

Candidates for Change in ``cpa-bioexcel-cwl``
---------------------------------------------

**Pre-migration application outline** -- see the 
`comparison <https://github.com/EMBL-EBI-TSI/cpa-bioexcel-cwl/compare/1dc1e987317eb313fa0a1f84b6548cafeb75d304...HEAD>`_
for the complete change set.

::

    cpa-bioexcel-cwl
    ├── manifest.json                            <-- Optional change (e.g. update version no.)  
    ├── ostack
    │   ├── ansible
    │   │   ├── ansible.cfg
    │   │   ├── playbook.yml                     <-- Mandatory change - syntactical change (+ "pip installer issue")
    │   │   ├── requirements.yml
    │   │   └── roles
    │   │       ├── cwl                          <-- Non-migratory "pip installer issue"
    │   │       │   └── tasks
    │   │       │       └── main.yml
    │   │       ├── docker
    │   │       │   └── tasks
    │   │       │       └── main.yaml
    │   │       ├── docker-venv
    │   │       │   └── tasks
    │   │       │       └── main.yaml
    │   │       ├── git-clone
    │   │       │   └── tasks
    │   │       │       └── main.yaml
    │   │       └── toil                         <-- Non-migratory "pip installer issue"
    │   │           └── tasks
    │   │               └── main.yml
    │   ├── deploy.sh                            <-- Mandatory change - terraform-related
    │   ├── destroy.sh                           <-- Mandatory change - terraform-related
    │   ├── state.sh                             <-- Mandatory change - terraform-related
    │   └── terraform                            <-- Mandatory change - add file "provider.tf"
    │       ├── firewall.tf
    │       ├── instance.tf                      <-- Mandatory change - terraform-related
    │       ├── keypair.tf
    │       ├── network.tf
    │       ├── output.tf
    │       ├── README.md
    │       ├── terraform.tfvars.example
    │       └── variables.tf                     <-- Mandatory change - syntactical change
    └── README.md                                <-- Optional change (e.g. update docs)

