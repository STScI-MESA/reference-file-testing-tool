*************************************
Using the Reference File Testing Tool
*************************************

The basic component of the Reference File Testing Tool is the ``test_reference_file`` function.  After
`installing the package <install.html>`_ it can be called from the command line with::

    test_reference_file /path/to/my/reference_file --data /path/to/some_uncal.fits

where the ``--data`` argument is some suitable level 1b JWST data.  This will run the JWST calibration pipeline on the
uncalibrated data overriding the default reference file with the one supplied

Alternatively, one can call the `~reftest.reftest.test_reference_file()` function from within a python session::

    >>> from reftest.reftest import test_reference_file
    >>> test_reference_file('/path/to/my/reference_file', '/path/to/some_uncal.fits')

Using a test data database
==========================

If you don't want to manually supply test data each time you test a reference file you can create a database of test
data for the Test Tool to automatically select from.

The database uses SQLite and is managed with the SQLAlchemy package.  You can create an empty database with the
`~reftest.db.create_test_data_db()` function.:

    >>> from reftest import db
    >>> db.create_test_data_db('/path/to/save.db')

You can then add data to the database with `~reftest.db.add_test_data()`:

    >>> db.add_test_data('/path/to/save.db', '/path/to/test/data.fits')

.. note::

    By default the `~reftest.db.add_test_data()` function will not add data if there is an existing data set with the
    same FITS keywords (INSTRUME, DETECTOR, CHANNEL, FILTER, PUPIL, BAND, GRATING, EXP_TYPE, READPATT, SUBARRAY) already
    in the database.  You can override this behaviour by passing the keyword argument ``force=True``.

With a database created you can run the Tool without specifying specific test data with::

    test_reference_file /path/to/my/reference_file

By default the Tool will test all matching data in the database; however, you can specify a maximum number of test data
to run.  For example, to use only the first match found::

    test_reference_file /path/to/my/reference_file --max-matches 1

