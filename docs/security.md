# 🛡️ Security & Privacy Architecture

### Data Handling & Persistence
* **No Persistence:** Portfolio-Lab is a "stateless" application. It does not use a database to store user-uploaded transactions.
* **In-Memory Processing:** All calculations are performed in-memory. Once your browser session is closed or refreshed, the uploaded data is cleared from the server's RAM.

### Reference Data Protection
* **AES-256 Encryption:** Core reference data (prices, FX rates, etc.) is stored in an encrypted volume using `pyAesCrypt`.
* **Environment Isolation:** The decryption key is managed via server-side environment variables, ensuring the credentials are never hard-coded or exposed in the repository.

### User Privacy
* **Local Processing:** While the app runs on Streamlit Cloud, the logic is designed to treat user data as transient.
* **No Tracking:** We do not implement third-party tracking or analytics that capture the contents of your portfolio.
