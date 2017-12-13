---
title: Backup and Restore
review:
    comment: ''
    date: ''
    status: ok
labels:
    - backup
confluence:
    ajs-parent-page-id: '16810080'
    ajs-parent-page-title: Monitoring and Maintenance
    ajs-space-key: ADMINDOC58
    ajs-space-name: Nuxeo Installation and Administration - 5.8
    canonical: Backup+and+Restore
    canonical_source: 'https://doc.nuxeo.com/display/ADMINDOC58/Backup+and+Restore'
    page_id: '16810078'
    shortlink: XoAAAQ
    shortlink_source: 'https://doc.nuxeo.com/x/XoAAAQ'
    source_link: /display/ADMINDOC58/Backup+and+Restore
history:
    - 
        author: Florent Guillaume
        date: '2014-06-26 16:00'
        message: ''
        version: '14'
    - 
        author: Solen Guitter
        date: '2013-10-30 10:22'
        message: Added links
        version: '13'
    - 
        author: Julien Carsique
        date: '2013-10-28 00:32'
        message: ''
        version: '12'
    - 
        author: Alain Escaffre
        date: '2013-10-27 22:34'
        message: ''
        version: '11'
    - 
        author: Alain Escaffre
        date: '2013-10-27 22:34'
        message: ''
        version: '10'
    - 
        author: Alain Escaffre
        date: '2013-10-27 22:33'
        message: ''
        version: '9'
    - 
        author: Alain Escaffre
        date: '2013-10-27 22:09'
        message: ''
        version: '8'
    - 
        author: Solen Guitter
        date: '2013-10-14 17:29'
        message: ''
        version: '7'
    - 
        author: Solen Guitter
        date: '2011-12-10 19:02'
        message: Migrated to Confluence 4.0
        version: '6'
    - 
        author: Solen Guitter
        date: '2011-12-10 19:02'
        message: ''
        version: '5'
    - 
        author: Julien Carsique
        date: '2011-06-06 15:52'
        message: ''
        version: '4'
    - 
        author: Julien Carsique
        date: '2011-06-06 15:49'
        message: ''
        version: '3'
    - 
        author: Florent Guillaume
        date: '2010-06-17 16:52'
        message: ''
        version: '2'
    - 
        author: Admin name placeholder
        date: '2010-03-01 00:58'
        message: ''
        version: '1'

---
## Backing Up

Nuxeo supports hot backup of your data.

If you have followed the [recommendations]({{page page='recommended-configurations'}}), then you have configured Nuxeo to use a production-safe database (instead of the default embedded H2) and have set a path for `nuxeo.data.dir` in your `nuxeo.conf`. In that case:

1.  Simply first backup your database (make a SQL dump),
2.  Then backup your data on filesystem.

Performing the backup in that order (the database first, then the filesystem) will ensure backup consistency.

If you didn't configure Nuxeo to use a database, then the default database is embedded in the data directory: Stop the server before backup.

If you didn't configure Nuxeo data directory (`nuxeo.data.dir` in `nuxeo.conf`), then the default path is `$TOMCAT/nxserver/data`.&nbsp;If you're not sure, look at the data directory value in the [Admin Center]({{page page='admin-center-overview'}}).

## Restoring

1.  Restore the database and data filesystem you had previously backed up.
2.  [Configure Nuxeo]({{page page='setup'}}) to use this database and data directory.
3.  Start Nuxeo.

## Additional Information

Two elements allow saving the filesystem once the database has been dumped:

*   When you add a document in the repository, [VCS]({{page space='glos' page='vcs'}}) computes the digest of the blob: it is this digest which is used as the filename of the document stored in the filesystem. That way, if a user uploads a different document but which has the same filename, the blob stored on filesystem won't be changed: a new blob with a different digest will be put in the blobstore.
*   Blobs are not deleted as soon as the document is removed from the repository.

These two points ensures that no data will be modified (or deleted) after dumping your database. Only creation could happen. So the backup of the filesystem will be consistent with the backup of the database.

**Some remarks about VCS**:

*   As VCS uses the digest of the blob, this ensures a document will be stored only once in the blobstore, even if it is uploaded several times.
*   As VCS doesn't delete blobs once a document is removed from the repository, you should run a clean-up regularly from the [Admin Center]({{page page='admin-center-overview'}}) in the menu **System Information** / **Repository binaries**.

&nbsp;