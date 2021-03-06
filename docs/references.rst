**********************************************************************
References
**********************************************************************

.. contents::

.. automethod:: pygit2.Repository.listall_reference_objects
.. automethod:: pygit2.Repository.listall_references
.. automethod:: pygit2.Repository.lookup_reference

Example::

    >>> all_refs = repo.listall_references()
    >>> master_ref = repo.lookup_reference("refs/heads/master")
    >>> commit = master_ref.get_object() # or repo[master_ref.target]


The Reference type
====================

.. autoclass:: pygit2.Reference

.. autoattribute:: pygit2.Reference.name
.. autoattribute:: pygit2.Reference.shorthand
.. autoattribute:: pygit2.Reference.target
.. autoattribute:: pygit2.Reference.type

.. automethod:: pygit2.Reference.set_target
.. automethod:: pygit2.Reference.delete
.. automethod:: pygit2.Reference.rename
.. automethod:: pygit2.Reference.resolve
.. automethod:: pygit2.Reference.peel
.. automethod:: pygit2.Reference.log

   Example::

      >>> branch = repository.lookup_reference("refs/heads/master")
      >>> branch.target = another_commit.id
      >>> committer = Signature('Cecil Committer', 'cecil@committers.tld')
      >>> branch.log_append(another_commit.id, committer,
                            "changed branch target using pygit2")

   This creates a reflog entry in ``git reflog master`` which looks like::

      7296b92 master@{10}: changed branch target using pygit2

   In order to make an entry in ``git reflog``, ie. the reflog for ``HEAD``, you
   have to get the Reference object for ``HEAD`` and call ``log_append`` on
   that.

.. automethod:: pygit2.Reference.get_object


The HEAD
====================

Example. These two lines are equivalent::

    >>> head = repo.lookup_reference('HEAD').resolve()
    >>> head = repo.head

.. autoattribute:: pygit2.Repository.head
.. autoattribute:: pygit2.Repository.head_is_detached
.. autoattribute:: pygit2.Repository.head_is_unborn

Branches
====================

Branches inherit from References, and additionally provide specialized
accessors for some unique features.

.. automethod:: pygit2.repository.Branches.create
.. automethod:: pygit2.repository.Branches.delete
.. automethod:: pygit2.repository.Branches.get
.. automethod:: pygit2.repository.Branches.__getitem__
.. automethod:: pygit2.repository.Branches.__iter__
.. automethod:: pygit2.repository.Branches.__contains__

Example::

    >>> # Listing all branches
    >>> branches_list = list(repo.branches)
    >>> # Local only
    >>> local_branches = list(repo.branches.local)
    >>> # Remote only
    >>> remote_branches = list(repo.branches.remote)

    >>> # Get a branch
    >>> branch = repo.branches['master']
    >>> other_branch = repo.branches['does-not-exist']  # Will raise a KeyError
    >>> other_branch = repo.branches.get('does-not-exist')  # Returns None

    >>> remote_branch = repo.branches.remote['upstream/feature']

    >>> # Create a local branch
    >>> new_branch = repo.branches.local.create('new-branch')

    >>> And delete it
    >>> repo.branches.delete('new-branch')


The Branch type
====================

.. autoattribute:: pygit2.Branch.branch_name
.. autoattribute:: pygit2.Branch.remote_name
.. autoattribute:: pygit2.Branch.upstream
.. autoattribute:: pygit2.Branch.upstream_name

.. automethod:: pygit2.Branch.rename
.. automethod:: pygit2.Branch.delete
.. automethod:: pygit2.Branch.is_head

The reference log
====================

Example::

    >>> head = repo.lookup_reference('refs/heads/master')
    >>> for entry in head.log():
    ...     print(entry.message)

.. autoclass:: pygit2.RefLogEntry
   :members:

Notes
====================

.. automethod:: pygit2.Repository.notes
.. automethod:: pygit2.Repository.create_note
.. automethod:: pygit2.Repository.lookup_note


The Note type
--------------------

.. autoattribute:: pygit2.Note.annotated_id
.. autoattribute:: pygit2.Note.id
.. autoattribute:: pygit2.Note.message
.. automethod:: pygit2.Note.remove
