/* ===================================
   CSS VARIABLE & RESET
   =================================== */
:root {
    --soft-pink: #F8D7DA;
    --cream: #FFF6EE;
    --nude-pink: #EFCFCF;
    --white: #FFFFFF;
    --text-dark: #333333;
    --text-light: #666666;
    --shadow-soft: 0 4px 20px rgba(248, 215, 218, 0.4);
    --shadow-medium: 0 8px 30px rgba(248, 215, 218, 0.5);
    --border-radius: 16px;
    --transition: all 0.3s ease;
}

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Poppins', sans-serif;
    background: var(--cream);
    color: var(--text-dark);
    line-height: 1.6;
    overflow-x: hidden;
}

/* ===================================
   AUTH PAGE
   =================================== */
.auth-page {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    background: linear-gradient(135deg, var(--soft-pink) 0%, var(--cream) 50%, var(--nude-pink) 100%);
}

.auth-container {
    width: 100%;
    max-width: 480px;
    padding: 20px;
    z-index: 10;
}

.auth-card {
    background: var(--white);
    border-radius: var(--border-radius);
    padding: 40px;
    box-shadow: var(--shadow-medium);
    animation: slideUp 0.6s ease;
}

.auth-header {
    text-align: center;
    margin-bottom: 30px;
}

.logo {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
    margin-bottom: 10px;
}

.logo i {
    font-size: 2rem;
    color: var(--soft-pink);
}

.logo h1 {
    font-size: 2rem;
    font-weight: 700;
    color: var(--text-dark);
}

.tagline {
    font-size: 1.2rem;
    color: var(--text-light);
    margin-bottom: 5px;
}

.subtitle {
    font-size: 0.9rem;
    color: var(--text-light);
    opacity: 0.8;
}

.auth-options {
    display: flex;
    flex-direction: column;
    gap: 20px;
    margin-bottom: 20px;
}

.auth-btn {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
    padding: 15px 30px;
    border-radius: 30px;
    text-decoration: none;
    font-weight: 600;
    font-size: 1rem;
    transition: var(--transition);
}

.login-btn {
    background: linear-gradient(135deg, var(--soft-pink), var(--nude-pink));
    color: var(--text-dark);
}

.register-btn {
    background: linear-gradient(135deg, #FFB6C1, #FFC0CB);
    color: var(--text-dark);
}

.auth-btn:hover {
    transform: translateY(-3px);
    box-shadow: var(--shadow-medium);
}

.divider {
    display: flex;
    align-items: center;
    text-align: center;
}

.divider::before,
.divider::after {
    content: '';
    flex: 1;
    border-bottom: 1px solid var(--nude-pink);
}

.divider span {
    padding: 0 15px;
    color: var(--text-light);
    font-size: 0.9rem;
}

.auth-footer {
    text-align: center;
    margin-top: 20px;
}

.auth-footer p {
    color: var(--text-light);
    font-size: 0.9rem;
}

.auth-footer a {
    color: var(--soft-pink);
    font-weight: 600;
    text-decoration: none;
}

.auth-footer a:hover {
    text-decoration: underline;
}

.features {
    display: flex;
    justify-content: center;
    gap: 15px;
    margin-top: 15px;
    flex-wrap: wrap;
}

.features span {
    font-size: 0.8rem;
    color: var(--text-light);
    display: flex;
    align-items: center;
    gap: 5px;
}

.features i {
    color: var(--soft-pink);
}

/* ===================================
   FORMS
   =================================== */
.auth-form {
    margin: 20px 0;
}

.form-group {
    margin-bottom: 20px;
}

.form-group label {
    display: block;
    margin-bottom: 8px;
    font-weight: 500;
    color: var(--text-dark);
    display: flex;
    align-items: center;
    gap: 8px;
}

.form-group label i {
    color: var(--soft-pink);
}

.form-group input,
.form-group select,
.form-group textarea {
    width: 100%;
    padding: 12px 15px;
    border: 2px solid
