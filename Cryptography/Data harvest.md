#Cybersecurity *Harvest now decrypt later principle*

Realistic near-term threat that does not require large quantum computer today:

Adversaries (nation-states) are recording encrypted internet traffic **now**. When large fault-tolerant quantum computer exists (estimated $10$-$20$ years), they will decrypt stored communications retroactively.

**Who is affected:**
- Diplomatic cables, classified communications, medical records, financial data - anything encrypted with [[RSA]] or [[ECDH]] today that needs to remain confidential in $2035$+
- Long-lived secrets: signing keys for software, root certificates, national ID infrastructure

**Why this matters for action today:** Post-quantum migration takes years. NIST finalized the $1st$ post-quantum standards in $2024$. Transition from [[RSA]] to post-quantum in TLS libraries, browsers, & servers is already underway but far from complete.