# Cookie Cutter - Risk Assignment Documentation

This document explains how Cookie Cutter – Security Checks assigns a risk score to each cookie. The score is based on several security attributes, and the total raw risk is scaled to a gauge from 0 (indicating minimal risk) to 10 (indicating a critical level of risk).

## Risk Factors and Their Values

For each cookie, the following criteria are evaluated:

1. **Secure Flag**  
   - **Missing Secure Flag:** +2 points  
   - **Present Secure Flag:** +0 points

2. **HttpOnly Flag**  
   - **Missing HttpOnly Flag:** +2 points  
   - **Present HttpOnly Flag:** +0 points

3. **SameSite Attribute**  
   - **Not Set or Set to "none" / "no_restriction":** +2 points  
   - **Set to "lax":** +1 point  
   - **Set to "strict":** +0 points

4. **Expiration**  
   - **Session Cookie (No expiration date):** +3 points  
   - **Persistent Cookie:**  
     - Expires in less than 1 day: +0 points  
     - Expires between 1 and 7 days: +1 point  
     - Expires in more than 7 days: +2 points

### Maximum Raw Risk

- The maximum possible raw risk is **9** points.
- The raw risk is then scaled to a 0–10 gauge using the formula:

  ### Risk Gauge = round((Raw Risk / 9) * 10)


## Examples

### Example 1: Critical-Risk Cookie

- **Attributes:**
- **Secure:** *False* (missing secure flag → +2)
- **HttpOnly:** *False* (missing HttpOnly flag → +2)
- **SameSite:** *Not set* (equivalent to "none" → +2)
- **Expiration:** *Session cookie* (no expiration date → +3)
- **Raw Risk Calculation:**
- 2 (secure) + 2 (HttpOnly) + 2 (SameSite) + 3 (Expiration) = **9**
- **Risk Gauge:**
- `round((9/9) × 10)` = **10**
- **Interpretation:**  
This cookie is assigned the maximum risk (10/10) and is considered critical.

---

### Example 2: Low-Risk Cookie

- **Attributes:**
- **Secure:** *True* (secure flag is present → +0)
- **HttpOnly:** *True* (HttpOnly flag is present → +0)
- **SameSite:** *Strict* (set to "strict" → +0)
- **Expiration:** *Persistent cookie* with an expiration date 30 days in the future (expires > 7 days → +2)
- **Raw Risk Calculation:**
- 0 (secure) + 0 (HttpOnly) + 0 (SameSite) + 2 (Expiration) = **2**
- **Risk Gauge:**
- `round((2/9) × 10)` ≈ **2**
- **Interpretation:**  
This cookie is considered low risk (2/10) due to its favorable security attributes.

---

### Example 3: Moderate-Risk Cookie

- **Attributes:**
- **Secure:** *True* (secure flag is present → +0)
- **HttpOnly:** *False* (missing HttpOnly flag → +2)
- **SameSite:** *Lax* (set to "lax" → +1)
- **Expiration:** *Persistent cookie* with an expiration date 3 days in the future (expires between 1 and 7 days → +1)
- **Raw Risk Calculation:**
- 0 (secure) + 2 (HttpOnly) + 1 (SameSite) + 1 (Expiration) = **4**
- **Risk Gauge:**
- `round((4/9) × 10)` ≈ **4**
- **Interpretation:**  
This cookie is moderately risky (4/10). Although it has a secure flag, the absence of HttpOnly and a lax SameSite setting contribute to a higher risk score.

---

## Summary

- **Risk Score Range:**  
- A risk gauge of **0** indicates a cookie that is considered less risky.
- A risk gauge of **10** indicates a cookie with critical risk.
- **Use in Cookie Cutter:**  
- The calculated risk gauge helps users quickly assess which cookies might require attention or removal.
- Cookies with higher scores should be reviewed for potential security concerns.

This documentation serves as a guide for understanding how Cookie Cutter evaluates cookie security, aiding users in maintaining a safer browsing environment.
