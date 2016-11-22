*******
Preface
*******

The most up to date version of this documentation can be found in **HTML** and **PDF** form on `ReadTheDocs <https://readthedocs.org/projects/dataselector-userguide/>`_.

.. index::
	single: Recommended user knowledge

Recommended User Knowledge
==========================

Users
-----

This user guide assumes that users of the Data Selector tool have:

* General IT experience including use of Microsoft Windows.
* Experience in the use of a relevant GIS application supported by the tool (currently ArcGIS or MapInfo), including selecting and querying features and attributes.
* An understanding of the data held in the Recorder6 database that has been configured for use by the Data Selector tool.


Administrators
--------------
It is recommended that a person within each organisation is designated as the tool and database administrator. This person should:

* Have an understanding and experience of IT systems management.
* Understand relational database structures.
* Have an good understanding of how to write Structure Query Language (SQL).
* Become familiar with how the Data Selector tool has been configured within the organisation.
* Have a good understanding of XML.

.. raw:: latex

   \newpage

.. index::
	single: Reading guide

Reading Guide
=============

This Preface explains a little about the Data Selector tool, the community of people who develop and use it, and the licensing conditions for using and distributing it. It also explains how to read this user guide.

:doc:`../introduction/introduction` \ explains why the Data Selector tool is needed, what it does and where it comes from.

:doc:`Extending Recorder6 <../recorder/recorder>` \ is a brief outline of how and how Recorder6 can be extended to enable the Data Selector tool to get the most out of your biological data.

:doc:`Setting up the tool <../setup/setup>` \ describes how to install and set up the Data Selector tool.

:doc:`Running the tool <../execute/execute>` \ describes how to run the Data Selector tool.

:doc:`Writing SQL <../sql/sql>` \ introduces how to write SQL queries to be used in the Data Selector tool.

:doc:`FAQs <../faq/faq>` \ has a list of commonly asked questions and their answers.

:doc:`../appendix/appendix` \ contains examples of the XML configuration files for MapInfo and ArcGIS, lists known issues with the tool and contains a copy of the GNU Free Documentation License v1.3 covering this guide.


.. index::
	single: Licensing

Licensing
=========

The code for the Data Selector tool is 'open source' and is released under the `GNU General Public License (GPL) v3 <http://www.gnu.org/licenses/gpl.html>`_. Users are free to install it on as many computers as they like, and to redistribute it according to the GPLv3 license.

This guide is released under the `GNU Free Documentation License (FDL) v1.3 <http://www.gnu.org/licenses/fdl.html>`_. Permission is granted to copy, distribute and/or modify this document under the terms of the license.

Please remember, however, that the tool cost a lot of money to develop and still requires further development and ongoing support. Hence any contributions towards costs would be gratefully received. Enquiries can be made via email to either `Hester <mailto:Hester@HesterLyonsConsulting.co.uk>`_ or `Andy <mailto:Andy@AndyFoyConsulting.co.uk>`_.


.. index::
	single: Useful links

Useful links
============

Related community links:

* Administrators (`ArcGIS <https://github.com/LERCAutomation/DataSelector--ArcObjects/releases/>`_ and `MapInfo <https://github.com/LERCAutomation/DataSelector-MapInfo/releases/>`_) - Release notes and installers for ArcGIS and MapInfo systems.
* Developers (`ArcGIS <https://github.com/LERCAutomation/DataSelector--ArcObjects>`_ and `MapInfo <https://github.com/LERCAutomation/DataSelector-MapInfo>`_) - Source code for the Data Selector Tool.
* Issues (`ArcGIS <https://github.com/LERCAutomation/DataSelector--ArcObjects/issues>`_ and `MapInfo <https://github.com/LERCAutomation/DataSelector-MapInfo/issues>`_) - Details of known issues and existing change requests.


.. index::
	single: Acknowledgements

Acknowledgements
================

Many thanks are due to all the LERCs in the south-east of England and their staff who have, and continue to, fund and contribute to the Data Selector tool.  It takes many developers, testers and users to build a truly useful tool (especially users who care enough to test new releases, report bugs and discuss feature requests).


.. raw:: latex

	\newpage

.. index::
	single: Conventions used in this user guide

Conventions used in this user guide
===================================

The following typographical conventions are used in this manual:

:kbd:`Ctrl-A`
	Indicates a key, or combination of keys, to press.

**Commit**
	Indicates a label, button or anything that appears in user interfaces.

**Tools... --> About**
	Indicates a menu choice, or a combination of menu choices, tab selections or GUI buttons.

:file:`C:\\Program Files (x86)\\MapInfo\\Professional`
	Indicates a filename or directory name.

.. tip::
	Tips can help save time or provide shortcuts.

.. note::
	Notes explain things in more detail or highlight important points.

.. caution::
	Warnings where users should pay attention.

