---
title: "Job Migrations"
subtitle: "Easier continuous delivery thanks to job migrations "
date: 2020-08-27T11:12:23+02:00
layout: "documentation"
menu: 
  main: 
    identifier: job-migrations
    parent: 'jobrunr-pro'
    weight: 27
---
<div style="text-align: center; margin: 1rem 0 3rem;">
    <a href="/en/try-jobrunr-pro/" class="btn btn-black btn-lg">
        <span>Try JobRunr Pro for free!</span>
    </a>
</div>

Do you have a lot of scheduled jobs planned in the future? And you need to do some refactoring? Just migrate your existing jobs to your new API and continue delivering working jobs with each deploy.

A job migration could not have been easier with the new migrations API.
<figure>

```java
JobRunrPro
    .configure()
    .useJobNotFoundConfiguration(usingStandardJobNotFoundConfiguration()
            .andMigrateScheduledJobsThatAreNotFound(
                    ((scheduledJobMatcher, scheduledJobUpdater) -> {
                        if (scheduledJobMatcher.matches(TestService.class, "doWorkThatDoesNotExist")) {
                            scheduledJobUpdater.setMethodName("doWork");
                        }
                    }),
                    ((scheduledJobMatcher, scheduledJobUpdater) -> {
                        if (scheduledJobMatcher.matches("i.dont.exist.Class")) {
                            scheduledJobUpdater.deleteJob();
                        }
                    })
    ))
```
<figcaption>

You can easily update jobs that do not exist anymore in your code via the scheduledJobUpdater class. Off-course, you can also add, remove and even update parameters from your old jobs.
</figcaption>
</figure>

These new migrations are also supported via the framework integrations using the `ScheduledJobThatDoNotExistAnymoreMigrations` bean.

<figure>

```java
@Singleton
public ScheduledJobThatDoNotExistAnymoreMigrations scheduledJobThatDoNotExistAnymoreMigrations() {
    return new ScheduledJobThatDoNotExistAnymoreMigrations(
        ((scheduledJobMatcher, scheduledJobUpdater) -> {
            if (scheduledJobMatcher.matches(TestService.class, "doWorkThatDoesNotExist")) {
                scheduledJobUpdater.setMethodName("doWork");
            }
        }),
        ((scheduledJobMatcher, scheduledJobUpdater) -> {
            if (scheduledJobMatcher.matches("i.dont.exist.Class")) {
                scheduledJobUpdater.deleteJob();
            }
        })
    );
}
```
<figcaption>

Updating existing jobs is possible in Spring, Micronaut and Quarkus by creating a ScheduledJobThatDoNotExistAnymoreMigrations bean.
</figcaption>
</figure>

> Note that the above example is just using a fraction of the API - the actual API to update jobs is much more extensive.

<div style="text-align: center; margin: 1rem 0 3rem;">
    <a href="/en/try-jobrunr-pro/" class="btn btn-black btn-lg">
        <span>Try JobRunr Pro for free!</span>
    </a>
</div>