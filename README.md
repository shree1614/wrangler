New Features
1. Byte Size Parser
The Byte Size Parser can parse various byte sizes such as KB, MB, and GB. It converts the input string into the canonical byte representation (in bytes).

Syntax:
Input format: [number][unit]

Example: 10KB, 150MB, 2GB

Example Usage:
java
Copy
Edit
ByteSize byteSize = new ByteSize("10KB");
System.out.println(byteSize.getBytes()); // Outputs: 10240 (bytes)

ByteSize byteSizeMB = new ByteSize("2MB");
System.out.println(byteSizeMB.getBytes()); // Outputs: 2097152 (bytes)
2. Time Duration Parser
The Time Duration Parser handles various time formats like milliseconds (ms), seconds (s), minutes (m), and hours (h). It converts the input into the canonical time representation (in nanoseconds).

Syntax:
Input format: [number][unit]

Example: 150ms, 3s, 1m, 2h

Example Usage:
java
Copy
Edit
TimeDuration timeDuration = new TimeDuration("150ms");
System.out.println(timeDuration.getNanoseconds()); // Outputs: 150000000 (nanoseconds)

TimeDuration timeDurationSec = new TimeDuration("3s");
System.out.println(timeDurationSec.getNanoseconds()); // Outputs: 3000000000 (nanoseconds)
3. Aggregate Stats Directive
The Aggregate Stats directive aggregates byte size and time duration values over a dataset. It calculates the total byte size and total time duration, then stores the aggregated results in the specified target columns.

Syntax:
java
Copy
Edit
aggregate-stats <size-column> <time-column> <size-target-column> <time-target-column>
Example Usage:
java
Copy
Edit
// Define a new AggregateStats directive
AggregateStats aggregateStats = new AggregateStats("sizeColumn", "timeColumn", "aggregatedSize", "aggregatedTime");

// Execute the aggregation
aggregateStats.execute(executionContext);

// Output aggregated results
System.out.println("Total Size: " + aggregatedSize); // Outputs the total size in MB
System.out.println("Total Time: " + aggregatedTime); // Outputs the total time in seconds
How to Use the Parsers and Directive
Step 1: Parsing Byte Size
To parse a byte size:

Instantiate the ByteSize class with a string input in the form of [number][unit].

Call the getBytes() method to retrieve the byte value.

java
Copy
Edit
ByteSize byteSize = new ByteSize("10MB");
long bytes = byteSize.getBytes();
System.out.println("10MB = " + bytes + " bytes");
Step 2: Parsing Time Duration
To parse a time duration:

Instantiate the TimeDuration class with a string input in the form of [number][unit].

Call the getNanoseconds() method to retrieve the time value in nanoseconds.

java
Copy
Edit
TimeDuration timeDuration = new TimeDuration("2s");
long nanoseconds = timeDuration.getNanoseconds();
System.out.println("2 seconds = " + nanoseconds + " nanoseconds");
Step 3: Using the Aggregate Stats Directive
Create an instance of AggregateStats with the required column names.

Execute the directive using the execute() method on an ExecutionContext.

The results will be stored in the specified target columns.

java
Copy
Edit
AggregateStats aggregateStats = new AggregateStats("sizeColumn", "timeColumn", "aggregatedSize", "aggregatedTime");
aggregateStats.execute(context);

// Access aggregated results
System.out.println("Total Size: " + aggregatedSize + " MB");
System.out.println("Total Time: " + aggregatedTime + " seconds");
Changes Made
Added Byte Size Parser for parsing KB, MB, and GB.

Added Time Duration Parser for parsing ms, s, m, and h.

Introduced Aggregate Stats Directive to aggregate byte sizes and time durations in a dataset.

Testing
To test the new functionality:

Add unit tests for ByteSize and TimeDuration classes to validate the parsing and conversion logic.

Ensure the AggregateStats directive correctly computes and stores the aggregated values.

Example unit tests are included for both parsers and the directive:

java
Copy
Edit
@Test
public void testByteSizeParsing() {
    ByteSize byteSize = new ByteSize("10KB");
    Assert.assertEquals(10240, byteSize.getBytes());
}

@Test
public void testTimeDurationParsing() {
    TimeDuration timeDuration = new TimeDuration("150ms");
    Assert.assertEquals(150000000, timeDuration.getNanoseconds());
}

@Test
public void testAggregateStats() {
    // Create mock rows with byte size and time duration
    // Run the aggregate-stats directive
    // Assert the aggregate values
}
Conclusion
With these new parsers and the aggregation directive, you can now handle byte size and time duration values more effectively and perform aggregation operations in your data pipelines. Make sure to update your projects with the latest version to take advantage of these features.# Data Prep

![cm-available](https://cdap-users.herokuapp.com/assets/cm-available.svg)
![cdap-transform](https://cdap-users.herokuapp.com/assets/cdap-transform.svg)
[![Build Status](https://travis-ci.org/cdapio/hydrator-plugins.svg?branch=develop)](https://travis-ci.org/cdapio/hydrator-plugins)
[![Coverity Scan Build Status](https://scan.coverity.com/projects/11434/badge.svg)](https://scan.coverity.com/projects/hydrator-wrangler-transform)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/io.cdap.wrangler/wrangler-core/badge.svg)](https://maven-badges.herokuapp.com/maven-central/io.cdap.wrangler/wrangler-core)
[![Javadoc](https://javadoc-emblem.rhcloud.com/doc/io.cdap.wrangler/wrangler-core/badge.svg)](http://www.javadoc.io/doc/io.cdap.wrangler/wrangler-core)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Join CDAP community](https://cdap-users.herokuapp.com/badge.svg?t=wrangler)](https://cdap-users.herokuapp.com?t=1)

A collection of libraries, a pipeline plugin, and a CDAP service for performing data
cleansing, transformation, and filtering using a set of data manipulation instructions
(directives). These instructions are either generated using an interative visual tool or
are manually created.

  * Data Prep defines few concepts that might be useful if you are just getting started with it. Learn about them [here](wrangler-docs/concepts.md)
  * The Data Prep Transform is [separately documented](wrangler-transform/wrangler-docs/data-prep-transform.md).
  * [Data Prep Cheatsheet](wrangler-docs/cheatsheet.md)

## New Features

More [here](wrangler-docs/upcoming-features.md) on upcoming features.

  * **User Defined Directives, also known as UDD**, allow you to create custom functions to transform records within CDAP DataPrep or a.k.a Wrangler. CDAP comes with a comprehensive library of functions. There are however some omissions, and some specific cases for which UDDs are the solution. Additional information on how you can build your custom directives [here](wrangler-docs/custom-directive.md).
    * Migrating directives from version 1.0 to version 2.0 [here](wrangler-docs/directive-migration.md)
    * Information about Grammar [here](wrangler-docs/grammar/grammar-info.md)
    * Various `TokenType` supported by system [here](../api/src/main/java/io/cdap/wrangler/api/parser/TokenType.java)
    * Custom Directive Implementation Internals [here](wrangler-docs/udd-internal.md)

  * A new capability that allows CDAP Administrators to **restrict the directives** that are accessible to their users.
More information on configuring can be found [here](wrangler-docs/exclusion-and-aliasing.md)

## Demo Videos and Recipes

Videos and Screencasts are best way to learn, so we have compiled simple, short screencasts that shows some of the features of Data Prep. Additional videos can be found [here](https://www.youtube.com/playlist?list=PLhmsf-NvXKJn-neqefOrcl4n7zU4TWmIr)

### Videos

  * [SCREENCAST] [Creating Lookup Dataset and Joining](https://www.youtube.com/watch?v=Nc1b0rsELHQ)
  * [SCREENCAST] [Restricted Directives](https://www.youtube.com/watch?v=71EcMQU714U)
  * [SCREENCAST] [Parse Excel files in CDAP](https://www.youtube.com/watch?v=su5L1noGlEk)
  * [SCREENCAST] [Parse File As AVRO File](https://www.youtube.com/watch?v=tmwAw4dKUNc)
  * [SCREENCAST] [Parsing Binary Coded AVRO Messages](https://www.youtube.com/watch?v=Ix_lPo-PDJY)
  * [SCREENCAST] [Parsing Binary Coded AVRO Messages & Protobuf messages using schema registry](https://www.youtube.com/watch?v=LVLIdWnUX1k)
  * [SCREENCAST] [Quantize a column - Digitize](https://www.youtube.com/watch?v=VczkYX5SRtY)
  * [SCREENCAST] [Data Cleansing capability with send-to-error directive](https://www.youtube.com/watch?v=aZd5H8hIjDc)
  * [SCREENCAST] [Building Data Prep from the GitHub source](https://youtu.be/pGGjKU04Y38)
  * [VOICE-OVER] [End-to-End Demo Video](https://youtu.be/AnhF0qRmn24)
  * [SCREENCAST] [Ingesting into Kudu](https://www.youtube.com/watch?v=KBW7a38vlUM)
  * [SCREENCAST] [Realtime HL7 CCDA XML from Kafka into Time Parititioned Parquet](https://youtu.be/0fqNmnOnD-0)
  * [SCREENCAST] [Parsing JSON file](https://youtu.be/vwnctcGDflE)
  * [SCREENCAST] [Flattening arrays](https://youtu.be/SemHxgBYIsY)
  * [SCREENCAST] [Data cleansing with send-to-error directive](https://www.youtube.com/watch?v=aZd5H8hIjDc)
  * [SCREENCAST] [Publishing to Kafka](https://www.youtube.com/watch?v=xdc8pvvlI48)
  * [SCREENCAST] [Fixed length to JSON](https://www.youtube.com/watch?v=3AXu4m1swuM)

### Recipes

  * [Parsing Apache Log Files](wrangler-demos/parsing-apache-log-files.md)
  * [Parsing CSV Files and Extracting Column Values](wrangler-demos/parsing-csv-extracting-column-values.md)
  * [Parsing HL7 CCDA XML Files](wrangler-demos/parsing-hl7-ccda-xml-files.md)

## Available Directives

These directives are currently available:

| Directive                                                              | Description                                                      |
| ---------------------------------------------------------------------- | ---------------------------------------------------------------- |
| **Parsers**                                                            |                                                                  |
| [JSON Path](wrangler-docs/directives/json-path.md)                              | Uses a DSL (a JSON path expression) for parsing JSON records     |
| [Parse as AVRO](wrangler-docs/directives/parse-as-avro.md)                      | Parsing an AVRO encoded message - either as binary or json       |
| [Parse as AVRO File](wrangler-docs/directives/parse-as-avro-file.md)            | Parsing an AVRO data file                                        |
| [Parse as CSV](wrangler-docs/directives/parse-as-csv.md)                        | Parsing an input record as comma-separated values                |
| [Parse as Date](wrangler-docs/directives/parse-as-date.md)                      | Parsing dates using natural language processing                  |
| [Parse as Excel](wrangler-docs/directives/parse-as-excel.md)                    | Parsing excel file.                                              |
| [Parse as Fixed Length](wrangler-docs/directives/parse-as-fixed-length.md)      | Parses as a fixed length record with specified widths            |
| [Parse as HL7](wrangler-docs/directives/parse-as-hl7.md)                        | Parsing Health Level 7 Version 2 (HL7 V2) messages               |
| [Parse as JSON](wrangler-docs/directives/parse-as-json.md)                      | Parsing a JSON object                                            |
| [Parse as Log](wrangler-docs/directives/parse-as-log.md)                        | Parses access log files as from Apache HTTPD and nginx servers   |
| [Parse as Protobuf](wrangler-docs/directives/parse-as-log.md)                   | Parses an Protobuf encoded in-memory message using descriptor    |
| [Parse as Simple Date](wrangler-docs/directives/parse-as-simple-date.md)        | Parses date strings                                              |
| [Parse XML To JSON](wrangler-docs/directives/parse-xml-to-json.md)              | Parses an XML document into a JSON structure                     |
| [Parse as Currency](wrangler-docs/directives/parse-as-currency.md)              | Parses a string representation of currency into a number.        |
| [Parse as Datetime](wrangler-docs/directives/parse-as-datetime.md)              | Parses strings with datetime values to CDAP datetime type        |
| **Output Formatters**                                                  |                                                                  |
| [Write as CSV](wrangler-docs/directives/write-as-csv.md)                        | Converts a record into CSV format                                |
| [Write as JSON](wrangler-docs/directives/write-as-json-map.md)                  | Converts the record into a JSON map                              |
| [Write JSON Object](wrangler-docs/directives/write-as-json-object.md)           | Composes a JSON object based on the fields specified.            |
| [Format as Currency](wrangler-docs/directives/format-as-currency.md)            | Formats a number as currency as specified by locale.             |
| **Transformations**                                                    |                                                                  |
| [Changing Case](wrangler-docs/directives/changing-case.md)                      | Changes the case of column values                                |
| [Cut Character](wrangler-docs/directives/cut-character.md)                      | Selects parts of a string value                                  |
| [Set Column](wrangler-docs/directives/set-column.md)                            | Sets the column value to the result of an expression execution   |
| [Find and Replace](wrangler-docs/directives/find-and-replace.md)                | Transforms string column values using a "sed"-like expression    |
| [Index Split](wrangler-docs/directives/index-split.md)                          | (_Deprecated_)                                                   |
| [Invoke HTTP](wrangler-docs/directives/invoke-http.md)                          | Invokes an HTTP Service (_Experimental_, potentially slow)       |
| [Quantization](wrangler-docs/directives/quantize.md)                            | Quantizes a column based on specified ranges                     |
| [Regex Group Extractor](wrangler-docs/directives/extract-regex-groups.md)       | Extracts the data from a regex group into its own column         |
| [Setting Character Set](wrangler-docs/directives/set-charset.md)                | Sets the encoding and then converts the data to a UTF-8 String   |
| [Setting Record Delimiter](wrangler-docs/directives/set-record-delim.md)        | Sets the record delimiter                                        |
| [Split by Separator](wrangler-docs/directives/split-by-separator.md)            | Splits a column based on a separator into two columns            |
| [Split Email Address](wrangler-docs/directives/split-email.md)                  | Splits an email ID into an account and its domain                |
| [Split URL](wrangler-docs/directives/split-url.md)                              | Splits a URL into its constituents                               |
| [Text Distance (Fuzzy String Match)](wrangler-docs/directives/text-distance.md) | Measures the difference between two sequences of characters      |
| [Text Metric (Fuzzy String Match)](wrangler-docs/directives/text-metric.md)     | Measures the difference between two sequences of characters      |
| [URL Decode](wrangler-docs/directives/url-decode.md)                            | Decodes from the `application/x-www-form-urlencoded` MIME format |
| [URL Encode](wrangler-docs/directives/url-encode.md)                            | Encodes to the `application/x-www-form-urlencoded` MIME format   |
| [Trim](wrangler-docs/directives/trim.md)                                        | Functions for trimming white spaces around string data           |
| **Encoders and Decoders**                                              |                                                                  |
| [Decode](wrangler-docs/directives/decode.md)                                    | Decodes a column value as one of `base32`, `base64`, or `hex`    |
| [Encode](wrangler-docs/directives/encode.md)                                    | Encodes a column value as one of `base32`, `base64`, or `hex`    |
| **Unique ID**                                                          |                                                                  |
| [UUID Generation](wrangler-docs/directives/generate-uuid.md)                    | Generates a universally unique identifier (UUID) .Recommended to use with Wrangler version 4.4.0 and above due to an important bug fix [CDAP-17732](https://cdap.atlassian.net/browse/CDAP-17732)             |
| **Date Transformations**                                               |                                                                  |
| [Diff Date](wrangler-docs/directives/diff-date.md)                              | Calculates the difference between two dates                      |
| [Format Date](wrangler-docs/directives/format-date.md)                          | Custom patterns for date-time formatting                         |
| [Format Unix Timestamp](wrangler-docs/directives/format-unix-timestamp.md)      | Formats a UNIX timestamp as a date                               |
| **DateTime Transformations**                                                    |                                                                  |
| [Current DateTime](wrangler-docs/directives/current-datetime.md)                | Generates the current datetime using the given zone or UTC by default|
| [Datetime To Timestamp](wrangler-docs/directives/datetime-to-timestamp.md)      | Converts a datetime value to timestamp with the given zone       |
| [Format Datetime](wrangler-docs/directives/format-datetime.md)                  | Formats a datetime value to custom date time pattern strings     |
| [Timestamp To Datetime](wrangler-docs/directives/timestamp-to-datetime.md)      | Converts a timestamp value to datetime                           |
| **Lookups**                                                            |                                                                  |
| [Catalog Lookup](wrangler-docs/directives/catalog-lookup.md)                    | Static catalog lookup of ICD-9, ICD-10-2016, ICD-10-2017 codes   |
| [Table Lookup](wrangler-docs/directives/table-lookup.md)                        | Performs lookups into Table datasets                             |
| **Hashing & Masking**                                                  |                                                                  |
| [Message Digest or Hash](wrangler-docs/directives/hash.md)                      | Generates a message digest                                       |
| [Mask Number](wrangler-docs/directives/mask-number.md)                          | Applies substitution masking on the column values                |
| [Mask Shuffle](wrangler-docs/directives/mask-shuffle.md)                        | Applies shuffle masking on the column values                     |
| **Row Operations**                                                     |                                                                  |
| [Filter Row if Matched](wrangler-docs/directives/filter-row-if-matched.md)      | Filters rows that match a pattern for a column                                         |
| [Filter Row if True](wrangler-docs/directives/filter-row-if-true.md)            | Filters rows if the condition is true.                                                  |
| [Filter Row Empty of Null](wrangler-docs/directives/filter-empty-or-null.md)    | Filters rows that are empty of null.                    |
| [Flatten](wrangler-docs/directives/flatten.md)                                  | Separates the elements in a repeated field                       |
| [Fail on condition](wrangler-docs/directives/fail.md)                           | Fails processing when the condition is evaluated to true.        |
| [Send to Error](wrangler-docs/directives/send-to-error.md)                      | Filtering of records to an error collector                       |
| [Send to Error And Continue](wrangler-docs/directives/send-to-error-and-continue.md) | Filtering of records to an error collector and continues processing                      |
| [Split to Rows](wrangler-docs/directives/split-to-rows.md)                      | Splits based on a separator into multiple records                |
| **Column Operations**                                                  |                                                                  |
| [Change Column Case](wrangler-docs/directives/change-column-case.md)            | Changes column names to either lowercase or uppercase            |
| [Changing Case](wrangler-docs/directives/changing-case.md)                      | Change the case of column values                                 |
| [Cleanse Column Names](wrangler-docs/directives/cleanse-column-names.md)        | Sanatizes column names, following specific rules                 |
| [Columns Replace](wrangler-docs/directives/columns-replace.md)                  | Alters column names in bulk                                      |
| [Copy](wrangler-docs/directives/copy.md)                                        | Copies values from a source column into a destination column     |
| [Drop Column](wrangler-docs/directives/drop.md)                                 | Drops a column in a record                                       |
| [Fill Null or Empty Columns](wrangler-docs/directives/fill-null-or-empty.md)    | Fills column value with a fixed value if null or empty           |
| [Keep Columns](wrangler-docs/directives/keep.md)                                | Keeps specified columns from the record                          |
| [Merge Columns](wrangler-docs/directives/merge.md)                              | Merges two columns by inserting a third column                   |
| [Rename Column](wrangler-docs/directives/rename.md)                             | Renames an existing column in the record                         |
| [Set Column Header](wrangler-docs/directives/set-headers.md)                     | Sets the names of columns, in the order they are specified       |
| [Split to Columns](wrangler-docs/directives/split-to-columns.md)                | Splits a column based on a separator into multiple columns       |
| [Swap Columns](wrangler-docs/directives/swap.md)                                | Swaps column names of two columns                                |
| [Set Column Data Type](wrangler-docs/directives/set-type.md)                    | Convert data type of a column                                    |
| **NLP**                                                                |                                                                  |
| [Stemming Tokenized Words](wrangler-docs/directives/stemming.md)                | Applies the Porter stemmer algorithm for English words           |
| **Transient Aggregators & Setters**                                    |                                                                  |
| [Increment Variable](wrangler-docs/directives/increment-variable.md)            | Increments a transient variable with a record of processing.     |
| [Set Variable](wrangler-docs/directives/set-variable.md)                        | Sets a transient variable with a record of processing.     |
| **Functions**                                                          |                                                                  |
| [Data Quality](wrangler-docs/functions/dq-functions.md)                         | Data quality check functions. Checks for date, time, etc.        |
| [Date Manipulations](wrangler-docs/functions/date-functions.md)                 | Functions that can manipulate date                               |
| [DDL](wrangler-docs/functions/ddl-functions.md)                                 | Functions that can manipulate definition of data                 |
| [JSON](wrangler-docs/functions/json-functions.md)                               | Functions that can be useful in transforming your data           |
| [Types](wrangler-docs/functions/type-functions.md)                              | Functions for detecting the type of data                         |

## Performance

Initial performance tests show that with a set of directives of high complexity for
transforming data, *DataPrep* is able to process at about ~106K records per second. The
rates below are specified as *records/second*. 

| Directive Complexity | Column Count |    Records |           Size | Mean Rate |
| -------------------- | :----------: | ---------: | -------------: | --------: |
| High (167 Directives) |      426      | 127,946,398 |  82,677,845,324 | 106,367.27 |
| High (167 Directives) |      426      | 511,785,592 | 330,711,381,296 | 105,768.93 |


## Contact

### Mailing Lists

CDAP User Group and Development Discussions:

* [cdap-user@googlegroups.com](https://groups.google.com/d/forum/cdap-user)

The *cdap-user* mailing list is primarily for users using the product to develop
applications or building plugins for appplications. You can expect questions from
users, release announcements, and any other discussions that we think will be helpful
to the users.

### IRC Channel

CDAP IRC Channel: [#cdap on irc.freenode.net](http://webchat.freenode.net?channels=%23cdap)

### Slack Team

CDAP Users on Slack: [cdap-users team](https://cdap-users.herokuapp.com)


## License and Trademarks

Copyright © 2016-2019 Cask Data, Inc.

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except
in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the
License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
either express or implied. See the License for the specific language governing permissions
and limitations under the License.

Cask is a trademark of Cask Data, Inc. All rights reserved.

Apache, Apache HBase, and HBase are trademarks of The Apache Software Foundation. Used with
permission. No endorsement by The Apache Software Foundation is implied by the use of these marks.
