
# Next Release

## Migrating

**Command-line interface (CLI).** Use `message-ix` as the program for all command-line operations:
- `message-ix copy-model` replaces `messageix-config`.
- `message-ix dl` replaces `messageix-dl`.
- `message-ix` also provides all the features of the :mod:`ixmp` CLI.

## All changes

- [#269](https://github.com/iiasa/message_ix/pull/269): Enforce 'year'-indexed columns as integers.
- [#256](https://github.com/iiasa/message_ix/pull/256): Update to use :obj:`ixmp.config` and improve CLI.
- [#255](https://github.com/iiasa/message_ix/pull/249): Add :mod:`message_ix.testing.nightly` and `nightly` CLI command group for slow-running tests.
- [#249](https://github.com/iiasa/message_ix/pull/249),
  [#259](https://github.com/iiasa/message_ix/pull/259): Build MESSAGE and MESSAGE_MACRO classes on ixmp model API; adjust Scenario.
- [#235](https://github.com/iiasa/message_ix/pull/236): Add a reporting tutorial.
- [#236](https://github.com/iiasa/message_ix/pull/236),
  [#242](https://github.com/iiasa/message_ix/pull/242),
  [#263](https://github.com/iiasa/message_ix/pull/263): Enhance reporting.
- [#232](https://github.com/iiasa/message_ix/pull/232): Add Westeros tutorial for modelling seasonality, update existing tutorials.

# v1.2.0

MESSAGEix 1.2.0 adds an option to set the commodity balance to strict equality,
rather than a supply > demand inequality. It also improves the support for
models with non-equidistant years.

Other improvements include an experimental reporting module, support for CPLEX
solver options via `Scenario.solve()`, and a reusable `message_ix.testing`
module.

Release 1.2.0 coincides with ixmp
[release 0.2.0](https://github.com/iiasa/ixmp/releases/tag/v0.2.0), which
provides full support for `Scenario.clone()` across platforms (database
instances), e.g. from a remote database to a local HSQL database; as well as
other improvements. See the ixmp release notes for further details.

## All changes

- [#161](https://github.com/iiasa/message_ix/pull/161): A feature for adding new periods to a scenario.
- [#205](https://github.com/iiasa/message_ix/pull/205): Implement required changes related to timeseries-support and cloning across platforms (see [ixmp:#142](https://github.com/iiasa/ixmp/pull/142)).
- [#196](https://github.com/iiasa/message_ix/pull/196): Improve testing by re-using :mod:`ixmp` apparatus.
- [#187](https://github.com/iiasa/message_ix/pull/187): Test for cumulative bound on emissions.
- [#182](https://github.com/iiasa/message_ix/pull/182): Fix cross-platform cloning.
- [#178](https://github.com/iiasa/message_ix/pull/178): Bugfix of the `PRICE_EMISSION` variable in models with non-equidistant period durations (#167)._
- [#176](https://github.com/iiasa/message_ix/pull/176): Add `message_ix.reporting` module.
- [#173](https://github.com/iiasa/message_ix/pull/173): The `solve` command now takes additional arguments when solving with CPLEX. The `cplex.opt` file is now generated on the fly during the solve command and removed after successfully solving.
- [#172](https://github.com/iiasa/message_ix/pull/172): Add option to set `COMMODITY_BALANCE` to equality.
- [#154](https://github.com/iiasa/message_ix/pull/154): Enable documentation build on ReadTheDocs.
- [#138](https://github.com/iiasa/message_ix/pull/138): Update documentation and tutorials.
- [#131](https://github.com/iiasa/message_ix/pull/131): Update clone function argument `scen` to `scenario` with planned deprecation of the former.


# v1.1.0

## Warnings

This patch introduces a few backwards-incompatible changes to database
management.

### Database Migration

If you see an error message like:

```
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
usr/local/lib/python2.7/site-packages/ixmp/core.py:81: in __init__
    self._jobj = java.ixmp.Platform("Python", dbprops)
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

self = <jpype._jclass.at.ac.iiasa.ixmp.Platform object at 0x7ff1a8e98410>
args = ('Python', '/tmp/kH07wz/test.properties')

    def _javaInit(self, *args):
        object.__init__(self)

        if len(args) == 1 and isinstance(args[0], tuple) \
           and args[0][0] is _SPECIAL_CONSTRUCTOR_KEY:
            self.__javaobject__ = args[0][1]
        else:
            self.__javaobject__ = self.__class__.__javaclass__.newClassInstance(
>               *args)
E           org.flywaydb.core.api.FlywayExceptionPyRaisable: org.flywaydb.core.api.FlywayException: Validate failed: Migration checksum mismatch for migration 1
E           -> Applied to database : 1588531206
E           -> Resolved locally    : 822227094
```

Then you need to update your local database. There are two methods to do so:

1. Delete it (you will lose all data and need to regenerate it). The default
   location is `~/.local/ixmp/localdb/`.
2. Manually apply the underlying migrations. This is not particularly easy, but
   allows you to save all your data. If you want help, feel free to get in
   contact on the
   [listserv](https://groups.google.com/forum/#!forum/message_ix).


### New Property File Layout

If you see an error message like:

```
usr/local/lib/python2.7/site-packages/jpype/_jclass.py:111: at.ac.iiasa.ixmp.exceptions.IxExceptionPyRaisable
---------------------------- Captured stdout setup -----------------------------
2018-11-13 08:15:17,410 ERROR at.ac.iiasa.ixmp.database.DbConfig:357 - missing property 'config.server.config' in /tmp/hhvE1o/test.properties
2018-11-13 08:15:17,412 ERROR at.ac.iiasa.ixmp.database.DbConfig:357 - missing property 'config.server.password' in /tmp/hhvE1o/test.properties
2018-11-13 08:15:17,412 ERROR at.ac.iiasa.ixmp.database.DbConfig:357 - missing property 'config.server.username' in /tmp/hhvE1o/test.properties
2018-11-13 08:15:17,413 ERROR at.ac.iiasa.ixmp.database.DbConfig:357 - missing property 'config.server.url' in /tmp/hhvE1o/test.properties
------------------------------ Captured log setup ------------------------------
core.py                     80 INFO     launching ixmp.Platform using config file at '/tmp/hhvE1o/test.properties'
_________________ ERROR at setup of test_add_spatial_multiple __________________

    @pytest.fixture(scope="session")
    def test_mp():
        test_props = create_local_testdb()

        # start jvm
        ixmp.start_jvm()

        # launch Platform and connect to testdb (reconnect if closed)
>       mp = ixmp.Platform(test_props)
```

Then you need to update your property configuration file. The old file looks like

```
config.name = message_ix_test_db@local
jdbc.driver.1 = org.hsqldb.jdbcDriver
jdbc.url.1 = jdbc:hsqldb:file:/path/to/database
jdbc.user.1 = ixmp
jdbc.pwd.1 = ixmp
jdbc.driver.2 = org.hsqldb.jdbcDriver
jdbc.url.2 = jdbc:hsqldb:file:/path/to/database
jdbc.user.2 = ixmp
jdbc.pwd.2 = ixmp
```

The new file should look like

```
config.name = message_ix_test_db@local
jdbc.driver = org.hsqldb.jdbcDriver
jdbc.url = jdbc:hsqldb:file:/path/to/database
jdbc.user = ixmp
jdbc.pwd = ixmp
```

## Updates

- [#202](https://github.com/iiasa/message_ix/pull/202): Added the "Development rule of thumb" section from the wiki and the Tutorial style guide to the Contributor guidelines. Tweaked some formatting to improve readibility
- [#113](https://github.com/iiasa/message_ix/pull/113): Upgrading to MESSAGEix 1.1: improved representation of renewables, share constraints, etc.
- [#109](https://github.com/iiasa/message_ix/pull/109): MACRO module added for initializing models to be solved with MACRO. Added scenario-based CI on circleci.
- [#99](https://github.com/iiasa/message_ix/pull/99): Fixing an error in the compuation of the auxiliary GAMS reporting variable `PRICE_EMISSION`
- [#89](https://github.com/iiasa/message_ix/pull/89): Fully implementing system reliability and flexibity considerations (cf. Sullivan)
- [#88](https://github.com/iiasa/message_ix/pull/88): Reformulated capacity maintainance constraint to ensure that newly installed capacity cannot be decommissioned within the same model period as it is built in
- [#84](https://github.com/iiasa/message_ix/pull/84): `message_ix.Scenariovintage_active_years()` now limits active years to those after the first model year or the years of a certain technology vintage
- [#82](https://github.com/iiasa/message_ix/pull/82): Introducing "add-on technologies" for mitigation options, etc.
- [#81](https://github.com/iiasa/message_ix/pull/81): Share constraints by mode added.
- [#80](https://github.com/iiasa/message_ix/pull/80): Share constraints by commodity/level added.
- [#78](https://github.com/iiasa/message_ix/pull/78): Bugfix: `message_ix.Scenario.solve()` uses 'MESSAGE' by default, but can be provided other model names
- [#77](https://github.com/iiasa/message_ix/pull/77): `rename()` function can optionally keep old values in the model (i.e., copy vs. copy-with-replace)
- [#74](https://github.com/iiasa/message_ix/pull/74): Activity upper and lower bounds can now be applied to all modes of a technology
- [#67](https://github.com/iiasa/message_ix/pull/67): Use of advanced basis in cplex.opt turned off by default to avoid conflicts with barrier method.
- [#65](https://github.com/iiasa/message_ix/pull/65): Bugfix for downloading tutorials. Now downloads current installed version by default.
- [#60](https://github.com/iiasa/message_ix/pull/60): Add basic ability to write and read model input to/from Excel
- [#59](https://github.com/iiasa/message_ix/pull/59): Added MacOSX CI support
