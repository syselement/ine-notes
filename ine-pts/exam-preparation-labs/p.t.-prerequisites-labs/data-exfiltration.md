# Data Exfiltration

## 🔬

The Kali OS GUI instance is web hosted on the INE website, where:

- You have exploited a vulnerable API endpoint and overwritten it with malicious code. This modification allows you to run commands on the server machine hosting the API, as a *low privilege user* (i.e. student). A sensitive flag file is kept in a zipped archive file in the *student user's home directory*.
- There is a monitor process running on the server machine that blocks most protocols *except HTTP protocol* (when using port **80**).
- The API endpoint is accessible at `demo.ine.local` domain.

*Objective*: Transfer the zipped archive to your Kali machine using HTTP protocol and retrieve the flag!

## SOLUTION

- 
