# Example 7.3

Suppose a particular system has a unit (device, component, assembly, and so on) that is tested for a vendor according to a required test regime that calls for 40 units to be tested for 2500 hours. The AF value for the required test is 10 000 hours per highly accelerated life test (HALT) cycle, and a HALT cycle has a duration of 1 hour. When the HALT is run, 1 of the 40 fails. The failure rate would be calculated as

Failure rate = λ = (1) / [(40)(2,500 cycles)(10,000 hours/cycle)] 10⁹ = 1 FIT

But you are not given a failure rate: instead, you are told that the FIT number is 50. Now you need to know the following to relate that FIT number to a failure rate (Ellerman 2012; NIST 2018):

FIT = 1/λ (7.1)

where 1 FIT = 1/10⁹ hours.

# Example 7.4

Given a FIT number of 50, you calculate the failure rate:

λ = 1/(50 FIT) = 10⁹/50 = 20,000,000 = 20 million hours!

Even though your research confirms that your calculation is correct, because the calculated rate of failure is so large, you decide to calculate an MTBF (mean time before failure) value using Eq. (7.1) (Abernethy 2006; RAC 2005; Speaks 2005; Weibull 2008):

MTBF = (total time)/(number of failures) (7.2)

But this does not help, because you do not know the total time, which you know is calculated using Eq. (7.3):

Total time = (# of tested units) * (test time) (7.3)

You also don’t know the number of tested units or the test time. So, you find another expression for MTBF

MTBF = 10⁹ / FIT (7.4)

which, for FIT = 50, is equal to the Example 7.4 calculated value for λ: 20 000 000 hours. Literature research reveals the following:

+   - In 1993, MTTF was defined as “mean time to first” failure (Seymour 1993), and the expression to calculate that value is given as
+     MTTF = (total time)/(number of failures)

+   - So, MTTF (circa 1993) is calculated the same way as MTBF in 2018, when B means before.

+   - You discover that MTBF is also defined as “mean time between failures” for a repairable product.

+   - MTTF is defined as “mean time to failure” (no reference to first) for a nonrepairable product.

+   - A relationship of MTBF to MTTF is defined as follows (Ellerman 2012):
+     MTBF = MTTF + MTTR (7.5)
+     where MTTR is defined as “mean time to repair.”