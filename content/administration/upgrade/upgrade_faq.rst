===================================
Upgrade: Frequently Asked Questions
===================================

.. _upgrade/faq/freeze:

.. spoiler:: Can the upgrade scripts be frozen?

   Odoo's standard upgrade scripts are not frozen when testing upgrades, as they evolve
   continuously.
   Everytime a new :ref:`test upgrade <upgrade-request-test>` is requested for a database, the
   latest version of the upgrade scripts is used to process it.

   However, when a :ref:`production database upgrade <upgrade-production>` is requested, if there is
   a successful test upgrade request **not older than 5 days**, the same version of the scripts is
   used to process it.

   This way, the upgrade runs with the same scripts that have already successfully processed the
   database.

.. _upgrade/faq/test-validity:

.. spoiler:: Can the validity of a test database be extended?

   No. A test database is meant only for testing purposes and is unrelated to the production
   database. If it expires, or if you want to test again, request a new one.

   There is no limit to the number of test databases that can be requested. Bear in mind that they
   are processed one at a time.

   Please request a fresh one before the production upgrade if your project has been running for a
   while, because the standard upgrade scripts and your production data both keep evolving.

.. _upgrade/faq/remove-custom-modules:

.. spoiler:: How do I remove custom modules during the upgrade?

   **Before the upgrade**

   If the custom modules are not needed anymore, uninstalling them on the Production database before
   the upgrade is the best option.

   **During the standard upgrade**

   If the custom modules are needed on the production database until it is upgraded but are not
   needed after.
   Please, create a ticket on the `Support page and selecting "Upgrade issue (in a test database)"
   <https://www.odoo.com/help?stage=migration>`_, indicating the custom modules to remove and we
   will make sure they are removed during the standard upgrade of the database.

   **After the standard upgrade**

   If the custom modules are meant to be removed after the standard upgrade, write an
   :doc:`upgrade script<../../developer/reference/upgrades/upgrade_scripts>` and remove the module
   with the :meth:`remove_module` function from the
   :doc:`../../developer/reference/upgrades/upgrade_utils` package.
   This allows running other operations before the module is removed, like moving its fields or
   models to another module, merging its data, and so on.

   For Odoo Online databases, simply uninstall the custom module from the :menuselection:`Apps`.

.. _upgrade/faq/downtime:

.. spoiler:: What is the downtime for a production upgrade?

   While Odoo does not provide a time estimate because downtime depends on many aspects like the
   size of the database, the version gap, and any custom code/modules it might have, you can gauge
   it using these steps:

   1. Request an :ref:`Upgraded Test Database <upgrade-request-test>`.

   2. Measure the duration of the request.

   3. For **Odoo.Sh** projects:

      a. Measure the time it takes to build the database on the staging branch: This includes the
         update of the custom modules that are installed and the execution of custom upgrade
         scripts, if any.
      b. Measure the time it takes to create a backup: For big databases, the creation of the backup
         prior to the upgrade can be significant and means a longer downtime when upgrading the
         production database. Create a manual backup of the production database and measure its time
         to better estimate the downtime.

   4. For **On Premise** databases:

      a. Measure the time it takes to deploy the test database, update the custom modules and run
         custom upgrade scripts, if any.
      b. Measure the time it takes to create a backup: We advise on creating a backup of the
         database before requesting the production upgrade, especially relevant in large databases.
      c. Measure the time it takes to create the database dump to send to the `upgrade platform
         <https://upgrade.odoo.com>`_ and the time it takes to restore the upgraded database, same
         as before, this is especially relevant only in large databases.

.. _upgrade/faq/sh_staging_backup:

.. spoiler:: Odoo.sh: On a staging branch, can I manually choose the backup to upgrade?

   No. The platform uses the **latest production daily automatic backup** and it's not possible to
   manually select a manual backup, neither from the production branch or any staging branch.

   If modifications done on the production database need to be included on the test upgrade, please
   wait until the new daily automatic backup is generated to request it.

   On the other hand, if modifications need to be done on the database before sending it to the
   Upgrade platform, do them on a local copy of the database and use the upload form or the command
   line to request the test upgraded database. The database obtained can then be imported on the
   staging branch for further testing.

.. _upgrade/faq/online_large_filestore:

.. spoiler:: Odoo Online: Filestore too large

   When dealing with Odoo Online databases that have a large filestore (more than 100 Gb), manual
   intervention by an Odoo agent might be needed to launch the test or production upgrades.

   When this is the case, a message is displayed on the upgrade widget of the database:

   .. image:: odoo-online-large-filestore.png
      :alt: The lasrge filestore "Upgrade" popup on Odoo online.

   .. warning::
      An error message **403 Forbidden: Filestore too large** could also be raised when requesting
      the upgrade.

   If this is blocking your upgrade request, please create a ticket on the `Support page and
   selecting "Upgrade issue (in a test database)" <https://www.odoo.com/help?stage=migration>`_ and
   an agent will contact you and help you trigger the upgrade for testing or production purposes.

   .. note::
      Reducing the size of the database and its filestore can help the upgrade run smoothly and
      avoid intervention from an Odoo agent, allowing you to have control over the upgrade and when
      its triggered.
