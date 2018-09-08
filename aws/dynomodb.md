# DynomoDB

* no sql databases
* need consistency, single-digit miliseconds latency at any scale
* great fit for mobile, web, gaming, ad-tech, IoT, and many other application
* stored on SSD storage
* spread across 3 geographically distinct data centers
* eventually consistency reads (default)
  * consistency across all copies of data is usually reached within a second. Repeating a read after a short time should return the updated data (best read performance)
* Strongly Consistency Reads
  * A strongly consistency read returns a result that reflects all writes that received a successful response prior to the read
* Pricing
  * Provisioned throughput capacity
    * write throughput 0.0065 per hour for every 10 units
    * read throughput 0.0065 per hour for every 50 units
  * storage costs of 0.25 GB per month
  * An example for pricing
