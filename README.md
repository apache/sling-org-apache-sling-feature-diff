[![Apache Sling](https://sling.apache.org/res/logos/sling.png)](https://sling.apache.org)

&#32;[![Build Status](https://ci-builds.apache.org/job/Sling/job/modules/job/sling-org-apache-sling-feature-diff/job/master/badge/icon)](https://ci-builds.apache.org/job/Sling/job/modules/job/sling-org-apache-sling-feature-diff/job/master/)&#32;[![Test Status](https://img.shields.io/jenkins/tests.svg?jobUrl=https://ci-builds.apache.org/job/Sling/job/modules/job/sling-org-apache-sling-feature-diff/job/master/)](https://ci-builds.apache.org/job/Sling/job/modules/job/sling-org-apache-sling-feature-diff/job/master/test/?width=800&height=600)&#32;[![Coverage](https://sonarcloud.io/api/project_badges/measure?project=apache_sling-org-apache-sling-feature-diff&metric=coverage)](https://sonarcloud.io/dashboard?id=apache_sling-org-apache-sling-feature-diff)&#32;[![Sonarcloud Status](https://sonarcloud.io/api/project_badges/measure?project=apache_sling-org-apache-sling-feature-diff&metric=alert_status)](https://sonarcloud.io/dashboard?id=apache_sling-org-apache-sling-feature-diff)&#32;[![JavaDoc](https://www.javadoc.io/badge/org.apache.sling/org.apache.sling.feature.diff.svg)](https://www.javadoc.io/doc/org.apache.sling/org-apache-sling-feature-diff)&#32;[![Maven Central](https://maven-badges.herokuapp.com/maven-central/org.apache.sling/org.apache.sling.feature.diff/badge.svg)](https://search.maven.org/#search%7Cga%7C1%7Cg%3A%22org.apache.sling%22%20a%3A%22org.apache.sling.feature.diff%22)&#32;[![feature](https://sling.apache.org/badges/group-feature.svg)](https://github.com/apache/sling-aggregator/blob/master/docs/group/feature.md) [![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)

This tool aims to provide to Apache Sling users an easy-to-use tool which is able to detect differences between different released version of the same Apache Sling Feature Model.

## Simple APIs

Given two different versions of the same `org.apache.sling.feature.Feature`, all we need to do is comparing them

```java
import static org.apache.sling.feature.diff.FeatureDiff.compareFeatures;

import org.apache.sling.feature.Feature
import org.apache.sling.feature.diff.DiffRequest;
import org.apache.sling.feature.diff.DiffRequest;

...

Feature previous = // somehow obtained
Feature current = // somehow obtained

DiffRequest diffRequest = new DiffRequest()
                          .setPrevious(previous)
                          .setCurrent(current)
                          .setResultId("org.apache.sling:org.apache.sling.diff:1.0.0");

Feature featureDiff = compareFeatures(diffRequest);
```

The resulting `featureDiff` is a new `Feature` instance which prototypes from `previous` and where necessary removals sections are populated and new elements may be added.

###Please note

The `FeatureDiff.compareFeatures(Feature, Feature)` rejects (aka throws an `IllegalArgumentException`) `Feature` inputs that:

 * are `null` (bien sûr);
 * refer exactly to the same `Feature`.

## Excluding sections

The `DiffRequest` data object can be configured in order to include/exclude one ore more Feature section(s), available are:

 * `bundles`
 * `configurations`
 * `extensions`
 * `framework-properties`

Users can simply add via the include/exclude methods the section(s) they are interested:

```java
DiffRequest diffRequest = new DiffRequest()
                          .setPrevious(previous)
                          .setCurrent(current)
                          .addIncludeComparator("bundles")
                          .addIncludeComparator("configurations")
                          .setResultId("org.apache.sling:org.apache.sling.diff:1.0.0");

Feature featureDiff = compareFeatures(diffRequest);
```
