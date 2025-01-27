---
title: Backup and Restore Monitoring
summary: An overview of the metrics to monitor backup and restore jobs in CockroachdB.
toc: true
docs_area: manage
---

CockroachDB includes metrics to monitor [backup](backup.html), [restore](restore.html), and [scheduled backup](create-schedule-for-backup.html) jobs. You can use monitoring integrations to alert when there are anomalies, such as backups that have failed or restore jobs encountering a retryable error.

Depending on whether you are using a {{ site.data.products.dedicated }} or {{ site.data.products.core }} cluster, you can use the following to monitor backup and restore metrics for your CockroachDB cluster:

- [Prometheus endpoint](#prometheus-endpoint): {{ site.data.products.core }}
- [Datadog integration](#datadog-integration): {{ site.data.products.dedicated }}

We recommend setting up monitoring to alert when anomalies occur. You can then use the following SQL statements to inspect details relating to schedules, jobs, and backups:

- [`SHOW SCHEDULES`](show-schedules.html)
- [`SHOW JOBS`](show-jobs.html)
- [`SHOW BACKUP`](show-backup.html)

For detail on [managed-service backups](../cockroachcloud/use-managed-service-backups.html) that Cockroach Labs stores for your {{ site.data.products.db }} cluster, see the **Backup and Restore** page in the Cloud Console.

{% include {{ page.version.version }}/backups/metrics-per-node.md %}

## Prometheus endpoint

You can access the [Prometheus endpoint](monitoring-and-alerting.html#prometheus-endpoint) (`http://<host>:<http-port>/_status/vars`) for backup and restore metrics with **{{ site.data.products.dedicated }} or {{ site.data.products.core }}** clusters.

See the [Monitor CockroachDB with Prometheus](monitor-cockroachdb-with-prometheus.html) tutorial for guidance on installing and setting up Prometheus and Alertmanager to track metrics.

### Available metrics

We recommend the following guidelines:

- Use the `schedules.BACKUP.last_completed_time` metric to monitor the specific backup job or jobs you would use to recover from a disaster.
- Configure alerting on the `schedules.BACKUP.last_completed_time` metric to watch for cases where the timestamp has not moved forward as expected.

Metric | Description 
-------+-------------
`schedules.BACKUP.failed` | The number of scheduled backup jobs that have failed. **Note:** A stuck scheduled job will not increment this metric.
`schedules.BACKUP.last_completed_time` | The Unix timestamp of the most recently completed scheduled backup specified as maintaining this metric. **Note:** This metric only updates if the schedule was created with the [`updates_cluster_last_backup_time_metric` option](create-schedule-for-backup.html#schedule-options).
<span class="version-tag">New in v23.1:</span> `schedules.BACKUP.protected_age_sec` | The age of the oldest [protected timestamp record](create-schedule-for-backup.html#protected-timestamps-and-scheduled-backups) protected by backup schedules.
<span class="version-tag">New in v23.1:</span>  `schedules.BACKUP.protected_record_count` | The number of [protected timestamp records](create-schedule-for-backup.html#protected-timestamps-and-scheduled-backups) held by backup schedules.
`schedules.BACKUP.started` | The number of scheduled backup jobs that have started.
`schedules.BACKUP.succeeded` | The number of scheduled backup jobs that have succeeded.
`schedules.round.reschedule_skip` | The number of schedules that were skipped due to a currently running job. A value greater than 0 indicates that a previous backup was still running when a new scheduled backup was supposed to start. This corresponds to the [`on_previous_running=skip`](create-schedule-for-backup.html#on-previous-running-option) schedule option.
`schedules.round.reschedule_wait` | The number of schedules that were rescheduled due to a currently running job. A value greater than 0 indicates that a previous backup was still running when a new scheduled backup was supposed to start. This corresponds to the [`on_previous_running=wait`](create-schedule-for-backup.html#on-previous-running-option) schedule option.
<span class="version-tag">New in v23.1:</span> `jobs.backup.currently_paused` | The number of backup jobs currently considered [paused](pause-job.html).
`jobs.backup.currently_running` | The number of backup jobs currently running in `Resume` or `OnFailOrCancel` state.
`jobs.backup.fail_or_cancel_retry_error` | The number of backup jobs that failed with a retryable error on their failure or cancelation process.
`jobs.backup.fail_or_cancel_completed` | The number of backup jobs that successfully completed their failure or cancelation process.
`jobs.backup.fail_or_cancel_failed` | The number of backup jobs that failed with a non-retryable error on their failure or cancelation process.
<span class="version-tag">New in v23.1:</span> `jobs.backup.protected_age_sec` | The age of the oldest [protected timestamp record](create-schedule-for-backup.html#protected-timestamps-and-scheduled-backups) protected by backup jobs.
<span class="version-tag">New in v23.1:</span> `jobs.backup.protected_record_count` | The number of [protected timestamp records](create-schedule-for-backup.html#protected-timestamps-and-scheduled-backups) held by backup jobs.
`jobs.backup.resume_failed` | The number of backup jobs that failed with a non-retryable error.
`jobs.backup.resume_retry_error` | The number of backup jobs that failed with a retryable error.
<span class="version-tag">New in v23.1:</span> `jobs.restore.currently_paused` | The number of restore jobs currently considered [paused](pause-job.html).
`jobs.restore.currently_running` | The number of restore jobs currently running in `Resume` or `OnFailOrCancel` state.
`jobs.restore.fail_or_cancel_failed` | The number of restore jobs that failed with a non-retriable error on their failure or cancelation process.
`jobs.restore.fail_or_cancel_retry_error` | The number of restore jobs that failed with a retryable error on their failure or cancelation process.
<span class="version-tag">New in v23.1:</span> `jobs.restore.protected_age_sec` | The age of the oldest [protected timestamp record](architecture/storage-layer.html#protected-timestamps) protected by restore jobs.
<span class="version-tag">New in v23.1:</span> `jobs.restore.protected_record_count` | The number of [protected timestamp records](architecture/storage-layer.html#protected-timestamps) held by restore jobs.
`jobs.restore.resume_completed` | The number of restore jobs that successfully resumed to completion.
`jobs.restore.resume_failed` | The number of restore jobs that failed with a non-retryable error.
`jobs.restore.resume_retry_error` | The number of restore jobs that failed with a retryable error.

## Datadog integration

To use the Datadog integration with your **{{ site.data.products.dedicated }}** cluster, you can:

- Export the following schedule backup metrics to Datadog using the [Cloud API](../cockroachcloud/cloud-api.html). To set this up, see [Export Metrics From a CockroachDB Dedicated Cluster](../cockroachcloud/export-metrics.html?filters=datadog-metrics-export).
- Access the Cloud Console **Monitoring** page to enable the integration. To set this up, see [Monitor CockroachDB Dedicated with Datadog](../cockroachcloud/tools-page.html#monitor-cockroachdb-dedicated-with-datadog).

### Available metrics in Datadog

Metric | Description 
-------+-------------
`schedules.BACKUP.succeeded` | The number of scheduled backup jobs that have succeeded.
`schedules.BACKUP.started` | The number of scheduled backup jobs that have started.
`schedules.BACKUP.last_completed_time` | The Unix timestamp of the most recently completed backup by a schedule specified as maintaining this metric.
`schedules.BACKUP.failed` | The number of scheduled backup jobs that have failed.

## See also

- [Third-Party Monitoring Integrations](third-party-monitoring-tools.html)
- [Manage a Backup Schedule](manage-a-backup-schedule.html)
- [Backup and Restore Overview](backup-and-restore-overview.html)
- [Backup Validation](backup-validation.html)