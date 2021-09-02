Changes
=======

<!--
    You should *NOT* be adding new change log entries to this file, this
    file is managed by towncrier. You *may* edit previous change logs to
    fix problems like typo corrections or such.

    To add a new change log entry, please refer
    https://pip.pypa.io/en/latest/development/contributing/#news-entries

    We named the news folder "changes".

    WARNING: Don't drop the last line!
-->

.. towncrier release notes start

20.09.29 (2021-09-02)
---------------------

### Fixes
* Add the missing extra requirements tag of SQLAlchemy to install greenlet and asyncpg correctly ([#470](https://github.com/lablup/backend.ai-manager/issues/470))
* Always set the scaling group when creating sessions to prevent use of non-allowed scaling groups ([#472](https://github.com/lablup/backend.ai-manager/issues/472))
* Fix a regression of `Agent.batch_load()` GraphQL resolver due to internal argument name changes ([#476](https://github.com/lablup/backend.ai-manager/issues/476))


20.09.28 (2021-08-23)
---------------------

### Features
* Add queryfilter/queryorder support for keypairs' (full_name, num_queries), users' (uuid), and kernels' (id, agent(s)) column. ([#464](https://github.com/lablup/backend.ai-manager/issues/464))

### Fixes
* Remove duplicate codes of `mount_map` check and add alias name check for `mount_map`. ([#461](https://github.com/lablup/backend.ai-manager/issues/461))
* Fix missing timestamp updates when terminating sessions ([#465](https://github.com/lablup/backend.ai-manager/issues/465))
* Include the exact list of missing/invalid vfolders when returning `VFolderNotFound` error ([#466](https://github.com/lablup/backend.ai-manager/issues/466))

### Miscellaneous
* Update package dependencies ([#462](https://github.com/lablup/backend.ai-manager/issues/462))


20.09.27 (2021-07-22)
---------------------

### Features
* Make `ilike` operator (equivalent to SQL's `ILIKE`) available in the queryfilter to allow case-insensitive string matching ([#458](https://github.com/lablup/backend.ai-manager/issues/458))

### Fixes
* Fix handling of value transforms with array values in queryfilter binary expressions ([#459](https://github.com/lablup/backend.ai-manager/issues/459))
* Let the idle timeout checkr skip batch-type sessions as they do not have any interaction whose absence are translated to idleness ([#460](https://github.com/lablup/backend.ai-manager/issues/460))


20.09.26 (2021-07-20)
---------------------

### Fixes
* Fix a critical bug due to a missing column from the select targets of join SQL queries in the new batched GQL object resolvers for `Group.by_user` and `ScalingGroup.by_group`, which only happens with non-admin user accounts ([#457](https://github.com/lablup/backend.ai-manager/issues/457))


20.09.25 (2021-07-20)
---------------------

### Fixes
* Fix a regression of session concurrency limit checks due to transaction retry refactoring in #429 ([#455](https://github.com/lablup/backend.ai-manager/issues/455))
* Partially revert and fix #454 which introduced default connection settings of deadlock/lock/transaction-idle timeouts for PostgreSQL, to make it working with AWS RDS ([#456](https://github.com/lablup/backend.ai-manager/issues/456))


20.09.24 (2021-07-19)
---------------------

### Features
* Add lock-related DB connection settings for better DB stability when the Manager does not release a lock and/or idle for a long time after acquiring a lock. ([#454](https://github.com/lablup/backend.ai-manager/issues/454))


20.09.23 (2021-07-19)
---------------------

### Fixes
* Fix missing order_key, order_asc -> order changes in paginated list GQL queries ([#453](https://github.com/lablup/backend.ai-manager/issues/453))


20.09.22 (2021-07-19)
---------------------

### Breaking Changes
* Removed never-used `order_key` and `order_asc` arguments in GraphQL pagination queries in favor of the new generic `order` argument ([#449](https://github.com/lablup/backend.ai-manager/issues/449))

### Features
* Now all paginated list GraphQL queries have optional `filter` and `order` arguments where the client may specify the filtering/ordering conditions using a simple mini-language expression ([#449](https://github.com/lablup/backend.ai-manager/issues/449))
* Add aiomonitor module for manager ([#450](https://github.com/lablup/backend.ai-manager/issues/450))
* Add `groups_by_name` GraphQL query to directly get group(s) from the given name ([#452](https://github.com/lablup/backend.ai-manager/issues/452))

### Fixes
* Apply missing batching of database queries for the `Group.scaling_groups` GraphQL field resolver. ([#451](https://github.com/lablup/backend.ai-manager/issues/451))
* Apply batching to user group resolution in GraphQL queries ([#452](https://github.com/lablup/backend.ai-manager/issues/452))


20.09.21 (2021-07-13)
---------------------

### Fixes
* Add a new GQL endpoint `/admin/gql` (in addition to existing `/admin/graphql`) which uses the standard-compliant response format ([#448](https://github.com/lablup/backend.ai-manager/issues/448))


20.09.20 (2021-07-13)
---------------------

### Fixes
* Handle failure of acquiring postgres advisory locks in the scheduler gracefully, by translating them as logged cancellations ([#444](https://github.com/lablup/backend.ai-manager/issues/444))
* Handle missing kernel log gracefully by adding a message about unavailability instead of panicking ([#445](https://github.com/lablup/backend.ai-manager/issues/445))
* Fix the regression of batch-type sessions by moving `startup_command` invocation to agents ([#447](https://github.com/lablup/backend.ai-manager/issues/447))


20.09.19 (2021-06-18)
---------------------

### Features
* Add an internal warning logs for excessive number of concurrent DB transactions ([#435](https://github.com/lablup/backend.ai-manager/issues/435))
* Add a common session environment variable `BACKENDAI_SESSION_NAME` for improved prompts and user acquaintance of which container they use ([#443](https://github.com/lablup/backend.ai-manager/issues/443))

### Fixes
* Rewrite internal database connection and transaction management for GraphQL query and mutation processing, which improves overall stability and performance ([#436](https://github.com/lablup/backend.ai-manager/issues/436))
* Handle missing root context gracefully with explicit warning during initialization of the intrinsic error monitor plugin ([#439](https://github.com/lablup/backend.ai-manager/issues/439))


20.09.18 (2021-05-17)
---------------------

### Features
* Add `PRE_AUTH_MIDDLEWARE` hook for cookie-based SSO plugins ([#420](https://github.com/lablup/backend.ai-manager/issues/420))

### Fixes
* Optimize read-only GraphQL queries to use read-only transaction isolation level, which greatly reduces the database loads when using GUI ([#431](https://github.com/lablup/backend.ai-manager/issues/431))


20.09.17 (2021-05-14)
---------------------

### Fixes
* Change the `KeyPair.num_queries` GQL field to use Redis instead of the `keypairs.num_queries` DB column to avoid excessive DB writes ([#421](https://github.com/lablup/backend.ai-manager/issues/421)) ([#425](https://github.com/lablup/backend.ai-manager/issues/425))
* Improve stability and synchronization of container-databse states
  - Now all DB transactions use the "SERIALIZABLE" isolation level with explicit retries.
  - Now DB transactions that includes only SELECT queries are marked as "read-only" so that
    the PostgreSQL engine could optimize concurrent access with the new isolation level.
    All future codes should use `beegin_readonly()` method from our own subclassed SQLAlchemy
    engine instance replacing all existing `db` context variables.
  - Remove excessive database updates due to keypair API query counts and kernel API query counts.
    The keypair API query count is re-written to use Redis with one month retention. (#421)
    Now just calling an API does not trigger updates in the PostgreSQL database.
  - Fix unnecessary database updates for agent heartbeats.
  - Split many update-only DB transactions into smaller units, such as resource recalculation.
  - Use PostgreSQL advisory locks to make the scheduling decision process as a critical section.
  - Fix some of variable binding issues with nested functions inside loops.
  - Apply event message coalescing to prevent event bursts (e.g., `DoScheduleEvent` fired after
    enqueueing new session requests) which hurts the database performance and potentially
    break the transaction isolation guarantees.
* Further refine the stability update with improved database transaction retries and the latest SQLAlchemy 1.4.x updates within the last month ([#429](https://github.com/lablup/backend.ai-manager/issues/429))
* Fix a regression that destroying a cluster session generates duplicate session termination events ([#430](https://github.com/lablup/backend.ai-manager/issues/430))

### Miscellaneous
* Temporarily pin `pytest-asyncio` to 0.14.0 due to regression of handling event loops for fixtures ([#423](https://github.com/lablup/backend.ai-manager/issues/423))


20.09.16 (2021-04-13)
---------------------

### Features
* Rewrite the session scheduler to avoid HoL blocking ([#415](https://github.com/lablup/backend.ai-manager/issues/415))
  - Skip over sessions in the queue if they fail to satisfy predicates for multiple retries -> 1st case of HoL blocking: a rogue pending session blocks everything in the same scaling group
  - You may configure the maximum number of retries in the `config/plugins/scheduler/fifo/num_retries_to_skip` etcd key.
  - Split the scheduler into two async loops for scheduling decision and session spawning by inserting "SCHEDULED" status between "PENDING" and "PREPARING" statuses -> 2nd case of HoL blocking: failure isolation with each task

### Fixes
* Adjust the firing rate of `DoPrepareEvent` to follow and alternate with the scheduler execution ([#418](https://github.com/lablup/backend.ai-manager/issues/418))


20.09.15 (2021-04-02)
---------------------

### Fixes
* Fix a regression of spawning multi-node cluster sessions due to DB API changes related to setting transaction isolation levels ([#416](https://github.com/lablup/backend.ai-manager/issues/416))


20.09.14 (2021-03-31)
---------------------

### Fixes
* Fix a missing reference fix for renaming of `gateway` to `manager.api` ([#409](https://github.com/lablup/backend.ai-manager/issues/409))
* Refactor the manager CLI initialization steps and promote `generate-keypair` as a regular `mgr` subcommand ([#411](https://github.com/lablup/backend.ai-manager/issues/411))
* Fix an internal API mismatch for our SQLAlchemy custom enum types ([#412](https://github.com/lablup/backend.ai-manager/issues/412))
* Fix a regression in session cancellation and kernel status updates after SQLAlchemy v1.4 upgrade ([#413](https://github.com/lablup/backend.ai-manager/issues/413))

### Miscellaneous
* Fix the examples for the storage proxy URL configurations in the `manager.config` module ([#410](https://github.com/lablup/backend.ai-manager/issues/410))
* Update sample configurations for etcd ([#414](https://github.com/lablup/backend.ai-manager/issues/414))


20.09.13 (2021-03-29)
---------------------

### Fixes
* Upgrade SQLAlchemy to v1.4 for native asyncio support and better transaction/concurrency handling ([#406](https://github.com/lablup/backend.ai-manager/issues/406))
* Add PostgreSQL version check to adjust connection arguments ([#407](https://github.com/lablup/backend.ai-manager/issues/407))
* Restructure the API layer by moving `ai.backend.gateway` modules to `ai.backend.manager.api` to make it consistent with other Backend.AI projects. ([#408](https://github.com/lablup/backend.ai-manager/issues/408))
  - The module field of API-related log entries will now have `ai.backend.manager.api.` prefix instead of `ai.backend.gateway.`
  - There is no changes in the TOML configuration but the `BACKEND_GATEWAY_NPROC` environment variable is renamed to `BACKEND_MANAGER_NPROC`.
  - You must update the license activator plugin (only the Enterprise edition users).
  - You may need to update SSO/webapp plugins to adapt with the new import paths.


20.09.12 (2021-03-19)
---------------------

### Fixes
* A follow-up hot fix for #400 ([#405](https://github.com/lablup/backend.ai-manager/issues/405))


20.09.11 (2021-03-18)
---------------------

### Fixes
* Refactor internal dict-based state variables to be statically typed using explicitly defined attr classes, including the nested `aiohttp.web.Application` objects, the context object of Graphene resolvers, and the context object of Click. ([#400](https://github.com/lablup/backend.ai-manager/issues/400))
* Idle timeout was not working since there was a type error on saving compute session's last_access time. ([#404](https://github.com/lablup/backend.ai-manager/issues/404))


20.09.10 (2021-03-08)
---------------------

### Fixes
* Scheduler for scaling group was entirely broken if only sub containers are running with main container is terminated. ([#403](https://github.com/lablup/backend.ai-manager/issues/403))


20.09.9 (2021-03-05)
--------------------

### Features
* Add BACKENDAI_KERNEL_IMAGE environment variable inside a compute container. ([#401](https://github.com/lablup/backend.ai-manager/issues/401))

### Fixes
* Unable to fetch virtual folders list for superadmins. ([#402](https://github.com/lablup/backend.ai-manager/issues/402))


20.09.8 (2021-02-22)
--------------------

### Fixes
* Update pyzmq to v22 series to reduce its wheel distribution size and fix a fork-safety bug introduced in v20. ([#399](https://github.com/lablup/backend.ai-manager/issues/399))


20.09.7 (2021-02-16)
--------------------

### BREAKING
* You must upgrade manager to 20.09.7+, agent to 20.09.5+, and common to 20.09.4+ altogether at once to make your cluster running correctly!

### Features
* Add `cloneable` parameter in list_folders, update_vfolder_options and clone function in class vfolder ([#393](https://github.com/lablup/backend.ai-manager/issues/393))
* Add `is_dir` parameter to the rename_file API function to provide renaming directories in Vfolders ([#397](https://github.com/lablup/backend.ai-manager/issues/397))

### Fixes
* Refactor out `gateway.events` and move common facilities to `common.events` to improve static typing of events and reduce possibility of serialization/deserialization bugs ([#392](https://github.com/lablup/backend.ai-manager/issues/392))
* Remove b-prefix in "push_background_task_events" function ([#394](https://github.com/lablup/backend.ai-manager/issues/394))
* Explicitly use `yaml.safe_load()` for all YAML loader invocations to prevent potential future mistakes to set an unsafe loader, whcih may allow remote code execution ([#395](https://github.com/lablup/backend.ai-manager/issues/395))
* Update uvloop to 0.15.1 for better Python 3.8/3.9 support (and drop Python 3.5/3.6 support) ([#398](https://github.com/lablup/backend.ai-manager/issues/398))


20.09.6 (2021-02-01)
--------------------

### Fixes
* Update dependencies including aiojobs, pytest, mypy, pyzmq, python-snappy, and PyYAML. ([#390](https://github.com/lablup/backend.ai-manager/issues/390))


20.09.5 (2021-01-20)
--------------------

### Features
* Add `usage` field to the `StorageVolume` graph object schema to provide the total capacity and usage ([#389](https://github.com/lablup/backend.ai-manager/issues/389))

### Fixes
* Improve daemon shutdown stability using aiotools v1.2 ([#386](https://github.com/lablup/backend.ai-manager/issues/386))
* Fix a regression in the idle timeout checker with session creation race fixes ([#387](https://github.com/lablup/backend.ai-manager/issues/387))
* Prevent indefinite hang of the scheduler while handling agent/container launch failures and fix resource calculation when processing multiple asynchronous errors ([#388](https://github.com/lablup/backend.ai-manager/issues/388))


20.09.4 (2021-01-08)
--------------------

### Features
* Add more indexes to reduce the latency for `kernels.cluster_role` involving queries ([#385](https://github.com/lablup/backend.ai-manager/issues/385))


20.09.3 (2021-01-04)
--------------------

### Fixes
* Fix reloading of new container registry configs when rescanning, which is a regression due to registry abstraction for Harbor v2 support ([#382](https://github.com/lablup/backend.ai-manager/issues/382))
* Fix `cluster_idx` for single-node sessions with only one container (`main0` should be `main1` to be consistent with multi-node sessions) ([#383](https://github.com/lablup/backend.ai-manager/issues/383))


20.09.2 (2020-12-30)
--------------------

### Fixes
* Fix the duplicate passed/failed predicate list in `status_data` ([#379](https://github.com/lablup/backend.ai-manager/issues/379))
* Fix a missing kernel ID field during cancel-termination upon failed creation of multi-container sessions ([#380](https://github.com/lablup/backend.ai-manager/issues/380))


20.09.1 (2020-12-28)
--------------------

### Fixes
* Put aiodocker in the default dependency, not testing. ([#377](https://github.com/lablup/backend.ai-manager/issues/377))
* Remove use of deprecated asyncio API (`asyncio.Task.{all_tasks,current_task}` -> `asyncio.{all_tasks,current_task}`) to run with Python 3.9 ([#378](https://github.com/lablup/backend.ai-manager/issues/378))


20.09.0 (2020-12-27)
--------------------

### Features
* Implement `max_container_per_session` keypair resource policy as a part of predicate checks ([#376](https://github.com/lablup/backend.ai-manager/issues/376))

### Fixes
* Additional stabilization of multi-node multi-container support ([#376](https://github.com/lablup/backend.ai-manager/issues/376))
  - Fix destruction of multi-container sessions when using multiple nodes
  - Adopt `MultiAgentError` to represent and handle partial failures of multi-container session creation


20.09.0rc3 (2020-12-26)
-----------------------

### Fixes
* Stabilize multi-container sessions further. ([#373](https://github.com/lablup/backend.ai-manager/issues/373))
  - Include predicate check results in `kernels.status_data` for better user-side diagnosis
  - Fix further race condition in the session-level creation routines, by adopting `session_creation_id` like the `kernel_creation_id` introduced in #374.
  - Remove the no longer used `agents.clusterized` column as it is no longer included in agent heartbeats after k8s branch refactoring.
  - Use dual DB connections to rollback agent-specific (resource) updates but to keep kernel-specific (status) updates when scheduling failures occur.
  - Fix a potential duplicate of DB cursors because use-after-free of a DB connection in the manager scheduler.
  - Relax row-level locks in scheduler in favor of the `REPEATABLE READ` isolation level.
  - Synchronize the candidate agent list when iterating over multiple kernels for a session in the multi-node mode, because they should now be updated
    according to the transaction savepoints.
  - Fix blocked proceeding to the next scaling group when a scheduling failure occurs in a scaling group.
  - Shield the DB queries in the events queue handlers and now interrupting the manager service works better.


20.09.0rc2 (2020-12-24)
-----------------------

### Fixes
* Fix races of kernel creation events by attaching a unique creation request ID to distinguish and catch the events by the caller manager instance ([#374](https://github.com/lablup/backend.ai-manager/issues/374))


20.09.0rc1 (2020-12-23)
-----------------------

### Fixes
* Unable to request watcher API due to incorrect reference to watcher endpoint. ([#370](https://github.com/lablup/backend.ai-manager/issues/370))
* Replace deprecated reference of kernels.c.role with kernels.c.cluster_role in legacy compute session list query. ([#371](https://github.com/lablup/backend.ai-manager/issues/371))
* Fix various bugs related to multi-node execution paths and provide richer error information with agent-side errors as a new JSON column `status_data` in the kernels table ([#372](https://github.com/lablup/backend.ai-manager/issues/372))


20.09.0b6 (2020-12-21)
----------------------

### Features
* Add a new API path for per-agent hardware metadata queries ([#366](https://github.com/lablup/backend.ai-manager/issues/366))


20.09.0b5 (2020-12-20)
----------------------

### Features
* Support destroying PREPARING/TERMINATING/ERROR sessions when forced parameter is delivered as True. ([#363](https://github.com/lablup/backend.ai-manager/issues/363))

### Fixes
* Fix a bug that prevents purge users and/or groups due to access to vfroot filesystem from manager directly, instead of delegating the tasks to storage-proxy. ([#361](https://github.com/lablup/backend.ai-manager/issues/361))
* Fix a race condition that sometimes corrupts the kernel status to be stuck at PREPARING even when kernels are successfully created ([#368](https://github.com/lablup/backend.ai-manager/issues/368))


20.09.0b4 (2020-12-19)
----------------------

### Fixes
* Fix a regression in image list graph queries due to a missing renaming of local_config ([#367](https://github.com/lablup/backend.ai-manager/issues/367))


20.09.0b3 (2020-12-18)
----------------------

### Fixes
* Update aiotools to v1.1.1 to fix a potential memory leak in long-lived taskgroup instances ([#362](https://github.com/lablup/backend.ai-manager/issues/362))
* Fix a hang-up issue when shutting down the gateway daemon with running service-port streaming sessions ([#365](https://github.com/lablup/backend.ai-manager/issues/365))

### Miscellaneous
* Update dependencies and adapt with the new pip resolver ([#359](https://github.com/lablup/backend.ai-manager/issues/359))
* Migrate to GitHub Actions completely for all CI jobs including tests and PyPI deployment ([#364](https://github.com/lablup/backend.ai-manager/issues/364))


20.09.0b2 (2020-12-07)
----------------------

### Features
* Add support for Harbor v2 and generalize internal abstractions of container registries, making it easier to add new registries in the future ([#357](https://github.com/lablup/backend.ai-manager/issues/357))

### Fixes
* Update fixtures to put the default registry as `cr.backend.ai` and remove unused ones ([#358](https://github.com/lablup/backend.ai-manager/issues/358))


20.09.0b1 (2020-12-02)
----------------------

### Fixes
* Improve handling of storage-manager invocation failures using a new `VFolderOperationFailed` API exception. ([#352](https://github.com/lablup/backend.ai-manager/issues/352))
* Fix missing replacement of `config_server` to `shared_config` in GraphQL context objects and references via `request.app['registry'].config_server` in some API handlers ([#354](https://github.com/lablup/backend.ai-manager/issues/354))
* Fix a regression of fetching `live_stat` GraphQL field values ([#355](https://github.com/lablup/backend.ai-manager/issues/355))
* Improve the error message for image not found errors when creating new sessions so that users could guess the reason quicklier ([#356](https://github.com/lablup/backend.ai-manager/issues/356))


20.09.0a4 (2020-11-16)
----------------------

### Features
* New idle checker to automatically kill sessions using various criteria ([#341](https://github.com/lablup/backend.ai-manager/issues/341))

### Fixes
* Implement and use a new global timer which works regardless of the number of manager instances to sustain periodic tasks such as session scheduling and idle checks ([#341](https://github.com/lablup/backend.ai-manager/issues/341))
* Replace legacy sess_id to session_name. ([#348](https://github.com/lablup/backend.ai-manager/issues/348))
* Fix intermittent resource warnings from aiopg by replacing all `fetchone()` with `first()` and `scalar()` that closes the database cursor automatically always ([#349](https://github.com/lablup/backend.ai-manager/issues/349))
* Update compute_plugins info from heartbeat for an ALIVE agent. ([#350](https://github.com/lablup/backend.ai-manager/issues/350))
* Fix broken signout. ([#353](https://github.com/lablup/backend.ai-manager/issues/353))


20.09.0a3 (2020-11-03)
----------------------

### Fixes
* Fix a regression in container-logs API since session name/ID matching generalization ([#346](https://github.com/lablup/backend.ai-manager/issues/346))
* Improve error handling in `gateway.wsproxy.TCPProxy` and use only aiohttp-supported WebSocket close codes to prevent client-side break-up ([#347](https://github.com/lablup/backend.ai-manager/issues/347))


20.09.0a2 (2020-10-30)
----------------------

### Features
* Add a new API `/session/{session_ref}/shutdown-service` for shutting down running in-container services ([#327](https://github.com/lablup/backend.ai-manager/issues/327))
* Add hooking point for AUTHORIZE with FIRST_COMPLETED requirement. ([#339](https://github.com/lablup/backend.ai-manager/issues/339))

### Fixes
* Unable to invite user to a vfolder, which is shared with another user. ([#340](https://github.com/lablup/backend.ai-manager/issues/340))
* Fix a regression of the manager status API due to internal etcd data structure changes, by returning the first-seen manager ID (for HA scenarios) to keep the compatibility of API semantics ([#342](https://github.com/lablup/backend.ai-manager/issues/342))
* Fix a regression in paginated list GraphQL query for vfolders due to missing code updates ([#343](https://github.com/lablup/backend.ai-manager/issues/343))
* Ensure removal of vfolders from the database when storage-proxy returns an error (e.g., not found) ([#344](https://github.com/lablup/backend.ai-manager/issues/344))
* Update dependencies (aiohttp~=3.7.0, trafaret~=2.1). ([#345](https://github.com/lablup/backend.ai-manager/issues/345))


20.09.0a1 (2020-10-06)
----------------------

### Breaking Changes
* The latest API version is now bumped to `v6.20200815`. ([#0](https://github.com/lablup/backend.ai-manager/issues/0))
* Configuration/DB changes are required for storage proxies ([#312](https://github.com/lablup/backend.ai-manager/issues/312))
  - To use vfolders, now the storage proxy must be installed and configured.
  - The "volumes" key in the etcd must include storage proxy configurations,
    as demonstrated in the `config/sample.volume.json` file.
  - All vfolder hosts are now in the format of "{proxy-name}:{volume-name}"
    where proxy-name is the key in the etcd and volume-name values are retrieved from
    the storage proxies at runtime.
  - All `allowed_vfolder_hosts` configurations in the database must be updated to
    the above new vfolder host format.
  - Clients should use the same vfolder host format when making API requests.

### Features
* Add support for multi-container sessions ([#217](https://github.com/lablup/backend.ai-manager/issues/217))
* Add a generic query filter expression language parser (`ai.backend.manager.models.minilang.queryfilter`) for GraphQL paginated list queries using [the Lark parser framework](https://github.com/lark-parser/lark) ([#305](https://github.com/lablup/backend.ai-manager/issues/305))
* Add support for storage proxies ([#312](https://github.com/lablup/backend.ai-manager/issues/312), [#337](https://github.com/lablup/backend.ai-manager/issues/337))
  - Storage proxies have a multi-backend architecture, so now storage-specific optimizations such as per-directory quota, performance measurements and fast metadata scanning all becomes available to Backend.AI users.
  - Offload and unify vfolder upload/download operations to storage proxies via the HTTP ranged queries and the tus.io protocol.
  - Support multiple storage proxies configured via etcd, and each storage proxy may provide multiple volumes in the mount points shared with agents.
  - Now the manager instances don't have mount points for the storage volumes, and mount/fstab management APIs skip the manager-side queries and manipulations.
* Include user information (full_name) in keypair gql query. ([#313](https://github.com/lablup/backend.ai-manager/issues/313))
* Add an endpoint that allows users to leave a shared virtual folder ([#317](https://github.com/lablup/backend.ai-manager/issues/317))
* Make the `mgr dbshell` command to be smarter to auto-detect the halfstack db container and use the container-provided psql command for maximum compatibility, with an optinal ability to set the container ID or name explicitly via `--psql-container` option and forward additional arguments to the psql command ([#318](https://github.com/lablup/backend.ai-manager/issues/318))
* Script to migrate /vroot/local to ex. /vroot/vfs structure according with new Storage Proxy implementation. ([#319](https://github.com/lablup/backend.ai-manager/issues/319))
* Make the maximum websocket message size configurable, which affects the operation of streaming APIs including service-port proxy ([#320](https://github.com/lablup/backend.ai-manager/issues/320))
* Add a new vfolder API to clone a vfolder and a property to vfolders for specifying cloneable or not ([#323](https://github.com/lablup/backend.ai-manager/issues/323), [#338](https://github.com/lablup/backend.ai-manager/issues/338))
* Add `quota` argument when creating vfolders (currently only supported in the xfs storage backend) ([#325](https://github.com/lablup/backend.ai-manager/issues/325))
* Add support for listing/updating/deleting/creating domain dotfiles and group dotfiles ([#329](https://github.com/lablup/backend.ai-manager/issues/329))
* Make vfolder's mkdir to accept and deliver parents and exist_ok option. ([#336](https://github.com/lablup/backend.ai-manager/issues/336))

### Fixes
* Prevent purging a user, group, and domain if they have active sessions and/or
  their bound resources, such as virtual folders, are being used by other sessions. ([#306](https://github.com/lablup/backend.ai-manager/issues/306))
* Fix conversion of string query params to int/bool ([#307](https://github.com/lablup/backend.ai-manager/issues/307))
* Do not create invitation if target user is already shared the folder. ([#308](https://github.com/lablup/backend.ai-manager/issues/308))
* Ignore missing tags in the Docker Hub when scanning the metadata for public Docker images, which happens after deleting images from the hub ([#309](https://github.com/lablup/backend.ai-manager/issues/309))
* Fix a regression of the GQL query to fetch the information about a single compute session ([#311](https://github.com/lablup/backend.ai-manager/issues/311))
* Return shared memory in preset list API. ([#314](https://github.com/lablup/backend.ai-manager/issues/314))
* Fix a regression in the server-side error logs query API due to a typo in the DB column name ([#315](https://github.com/lablup/backend.ai-manager/issues/315))
* Fix ErrorMonitor plugin instance to match the updates in plugin class of backend.ai-common ([#316](https://github.com/lablup/backend.ai-manager/issues/316))
* Deduplicate explicitly raised internal server errors in the daemon logs ([#322](https://github.com/lablup/backend.ai-manager/issues/322))
* Log detailed error messages when graphQl exceptions are raised. ([#324](https://github.com/lablup/backend.ai-manager/issues/324))
* Fix a regression of statistics API due to a wrong default value type ([#326](https://github.com/lablup/backend.ai-manager/issues/326))
* Fix vfolder mounts for compute sessions using agent host paths provided by the storage proxy ([#328](https://github.com/lablup/backend.ai-manager/issues/328))
* Hand over recursive parameter to storage proxy when deleting files. ([#330](https://github.com/lablup/backend.ai-manager/issues/330))
* Validate service port declarations before starting the session ([#333](https://github.com/lablup/backend.ai-manager/issues/333))
* Improve pickling of wrapped HTTP exceptions (`BackendError` and its derivatives) ([#334](https://github.com/lablup/backend.ai-manager/issues/334))
* Show the container name when using the `mgr dbshell` cli command ([#335](https://github.com/lablup/backend.ai-manager/issues/335))


20.03.0 (2020-07-28)
--------------------

### Features
* Add purge mutations for users, groups, and domains. ([#302](https://github.com/lablup/backend.ai-manager/issues/302))

### Fixes
* Raise explicit exception when no keypair is found during login. ([#297](https://github.com/lablup/backend.ai-manager/issues/297))
* Adds context for the call to from_row in simple_db_mutate_returning_item. ([#300](https://github.com/lablup/backend.ai-manager/issues/300))
* The session creation API now returns the session UUID as part of its response, in addition to the meaningless client-provided session alias (name) ([#303](https://github.com/lablup/backend.ai-manager/issues/303))

### Miscellaneous
* Return explicit message if user needs verification during login. ([#301](https://github.com/lablup/backend.ai-manager/issues/301))


20.03.0rc1 (2020-07-23)
-----------------------

### Breaking Changes
* Replace user's boolean-based `is_active` field to a enum-based `status` field to represent multiple user statuses with lifecycle state transitions, changing the signup API and related DB columns.
* Change the argument type of `user_id` in `user_from_uuid` GraphQL query to the generic `ID` ([#299](https://github.com/lablup/backend.ai-manager/issues/299))

### Fixes
* Fix misuse of a legacy API in the `etcd move-subtree` manager command, which has unintentionally flattened nested dicts when inserting the data to the new location ([#295](https://github.com/lablup/backend.ai-manager/issues/295))
* Mask sensitive keys when logging API parameters for debugging ([#296](https://github.com/lablup/backend.ai-manager/issues/296))
* A set of API behavior fixes ([#299](https://github.com/lablup/backend.ai-manager/issues/299))
  - Fix missing conversion of `user_id` argument to UUID type when resolving the `user_from_uuid` GraphQL query
  - Exclude cancelled sessions from the active session count in the manager status API
  - Fix missing update for user's `status_info` as "admin-requested" when marked as deleted by administrators

### Miscellaneous
* Refactor session usage api. ([#298](https://github.com/lablup/backend.ai-manager/issues/298))


20.03.0b2 (2020-07-02)
----------------------

### Breaking Changes
* Apply the plugin API v2 -- all stats/error/webapp/hook plugins must be updated along with the manager ([#291](https://github.com/lablup/backend.ai-manager/issues/291))

### Features
* Group-shared vfolders named ".local" (while other dot-prefixed names are not allowed for group vfolders) now have per-user subdirectories (using user UUIDs) for per-user persistent package installation reusable across different sessions ([#224](https://github.com/lablup/backend.ai-manager/issues/224))
* Download a directory from vfolder as an archive. ([#278](https://github.com/lablup/backend.ai-manager/issues/278))
* Execute POST_SIGNUP hook after sign up. ([#286](https://github.com/lablup/backend.ai-manager/issues/286))
* Pass Accept-Language header to post-signup hook. ([#287](https://github.com/lablup/backend.ai-manager/issues/287))
* Add a feature to kick off user who is shared virtual folder. ([#288](https://github.com/lablup/backend.ai-manager/issues/288))
* Add a global announcement get/set API which stores an arbitrary string in the "manager/announcement" etcd key. ([#289](https://github.com/lablup/backend.ai-manager/issues/289))
* Add option to reserve batch session. ([#290](https://github.com/lablup/backend.ai-manager/issues/290))
* Add modified_at column to users and keypairs table. ([#293](https://github.com/lablup/backend.ai-manager/issues/293))
* Add super-admin APIs to temporarily exclude specific agents from scheduling and include later by changing the `schedulable` column of the `agents` table. ([#294](https://github.com/lablup/backend.ai-manager/issues/294))

### Fixes
* Destroying sessions now returns periodically collected statistics instead of last-moment statistics ([#279](https://github.com/lablup/backend.ai-manager/issues/279))
* Apply batching to statistics synchroniztion to minimize time used inside DB transactions by batching and splitting transaction blocks ([#280](https://github.com/lablup/backend.ai-manager/issues/280))
* Add fallback if any keys under last_stat have None value during statistics calculation. ([#282](https://github.com/lablup/backend.ai-manager/issues/282))
* Fix the responses for `compute_session` GQL query when there are no or multiple matching sessions ([#283](https://github.com/lablup/backend.ai-manager/issues/283))
* ([#284](https://github.com/lablup/backend.ai-manager/issues/284))
  - Fix too many scheduler invocation with stale periodic scheduling ticks after manager restarts/maintenance
  - Split DB transactions for schedulers as fine-grained, small-sized transactions to cope with a large number of pending sessions
* Ensure resolving of kernel IDs before doing complex operations in `AgentRegistry` to prevent potential races against the unique constraints about kernel statuses and user-defined session names under heavy loads ([#292](https://github.com/lablup/backend.ai-manager/issues/292))


20.03.0b1 (2020-05-12)
----------------------

### Breaking Changes
* Now it runs on Python 3.8 or higher only.
* Rename all APIs to use "session" instead of "kernel" ([#216](https://github.com/lablup/backend.ai-manager/issues/216))
* API v5 support
  - All session-related GraphQL schema is rewritten to reflect multi-container sessions and kernel -> session parameter and API renamings. ([#263](https://github.com/lablup/backend.ai-manager/issues/263))

### Features
* Un-numbered features
  - Now all CLI commands are accessible via ``backend.ai mgr``. [(lablup/backend.ai#101)](https://github.com/lablup/backend.ai/issues/101)
  - Add more convenient manager CLI commands: `etcd quote`, `etcd unquote`, `etcd move-subtree`
  - Add `--short` and `--installed` options to the `etcd list-images` command.
  - Add a minimum-occupied slot first scheduler.
* Now our manager-to-agent RPC uses [Callosum](https://github.com/lablup/callosum) instead of aiozmq, supporting Python 3.8 natively. ([#209](https://github.com/lablup/backend.ai-manager/issues/209))
* Add vfolder large-file upload APIs using the tus.io protocol. ([#210](https://github.com/lablup/backend.ai-manager/issues/210))
* User-customizable per-scaling-group session queue scheduler plugins and three intrinsic plugins: FIFO, LIFO, and the DRF (dominant resource fairness) scheduler. ([#212](https://github.com/lablup/backend.ai-manager/issues/212))
* User-manageable session templates written in YAML to reuse session creation parameters. ([#213](https://github.com/lablup/backend.ai-manager/issues/213))
* ResourceSlots are now more permissive so that agents with different sets of accelerator plugins can now coexist in a single cluster. ([#214](https://github.com/lablup/backend.ai-manager/issues/214))
* Add pre-open service ports to allow user-written applications listening on a container port and make such ports accessible via the stream proxy API. ([#221](https://github.com/lablup/backend.ai-manager/issues/221))
* Auto-fill the minimum resource slots for intrinsic slots only to allow execution of sessions with different set of accelerators simultaneously. ([#234](https://github.com/lablup/backend.ai-manager/issues/234))
* Error logging API and an intrinsic plugin to store all unhandled exceptions in agents and managers.
  (Agent exceptions are passed via the event bus) ([#235](https://github.com/lablup/backend.ai-manager/issues/235))
* Reduce possibility of aioredlock locking errors. ([#236](https://github.com/lablup/backend.ai-manager/issues/236))
* Favor CPU agents for session creation requests without accelerators ([#241](https://github.com/lablup/backend.ai-manager/issues/241))
* Add user-config APIs to update/get keypair bootstrap script ([#253](https://github.com/lablup/backend.ai-manager/issues/253))
* Accept session UUIDs and their prefixes in addition to client-specified IDs (names) ([#258](https://github.com/lablup/backend.ai-manager/issues/258))
* API v5 support ([#263](https://github.com/lablup/backend.ai-manager/issues/263))
  - Add full implementation of the background-task tracking API
  - Apply background-task tracking API to the image rescanning API
  - Add paginated UserList and KeyPairList GraphQL queries
* Extend vfolder to have usage mode and innate permission ([#265](https://github.com/lablup/backend.ai-manager/issues/265))
* Add shared memory column to resource preset ([#267](https://github.com/lablup/backend.ai-manager/issues/267))

### Fixes
* Fix regression of the docker registry list fetch API (`/config/docker-registries`) ([#259](https://github.com/lablup/backend.ai-manager/issues/259))
* Fix errors on fetching server logs and vfolder list ([#264](https://github.com/lablup/backend.ai-manager/issues/264))
* Remove unwanted empty key while setting etcd config with nested dict. ([#275](https://github.com/lablup/backend.ai-manager/issues/275))

### Miscellaneous
* Internally refactored the main function for easier writing of future unit tests by composing different resource cleanup contexts in a modular way, using aiohttp's cleanup contexts.
* Now all changelogs are managed using [towncrier](https://github.com/twisted/towncrier) and migrated to Markdown syntax. ([#262](https://github.com/lablup/backend.ai-manager/issues/262))
* Refactor `batch_result()` and `batch_multiresult()` used for optimizing SQL-based GraphQL queries ([#263](https://github.com/lablup/backend.ai-manager/issues/263))
* Return NaN when group-level resource status is not allowed to the client. ([#268](https://github.com/lablup/backend.ai-manager/issues/268))
* Update flake8 to a prerelease version supporting Python 3.8 syntaxes ([#277](https://github.com/lablup/backend.ai-manager/issues/277))
