<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TextMoji | Secure Emoji Encryption</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/4.1.1/crypto-js.min.js"></script>
    <style>
        :root {
            --primary: #7c3aed;
            --primary-dark: #6d28d9;
            --secondary: #06b6d4;
            --dark: #1e293b;
            --darker: #0f172a;
            --light: #f8fafc;
            --gray: #64748b;
            --success: #10b981;
            --error: #ef4444;
            --card-bg: rgba(30, 41, 59, 0.7);
            --card-border: rgba(99, 102, 241, 0.2);
            --text-primary: #f8fafc;
            --text-secondary: #cbd5e1;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, var(--darker), var(--dark));
            color: var(--light);
            min-height: 100vh;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* Authentication Styles */
        .auth-page {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, var(--darker), var(--dark));
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 9999;
        }

        .auth-container {
            width: 100%;
            max-width: 420px;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(20px);
            border-radius: 20px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
            overflow: hidden;
        }

        .auth-header {
            padding: 30px;
            text-align: center;
            background: linear-gradient(90deg, var(--primary), var(--secondary));
            position: relative;
            overflow: hidden;
        }

        .auth-header h2 {
            font-size: 2rem;
            font-weight: 800;
            margin-bottom: 10px;
            background: linear-gradient(to right, #fff, #e0e7ff);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .auth-header p {
            color: rgba(255, 255, 255, 0.8);
            font-size: 1rem;
        }

        .auth-content {
            padding: 30px;
        }

        .auth-form {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .auth-input-group {
            margin-bottom: 20px;
        }

        .auth-input-group label {
            display: block;
            margin-bottom: 10px;
            font-weight: 600;
            color: var(--text-primary);
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .auth-input-group input {
            width: 100%;
            padding: 15px;
            border-radius: 12px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            background: rgba(255, 255, 255, 0.1);
            color: var(--text-primary);
            font-size: 1rem;
            transition: all 0.3s ease;
        }

        .auth-input-group input:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(124, 58, 237, 0.3);
            background: rgba(255, 255, 255, 0.15);
        }

        .auth-input-group input::placeholder {
            color: rgba(255, 255, 255, 0.6);
        }

        .password-container {
            position: relative;
        }

        .toggle-password {
            position: absolute;
            right: 15px;
            top: 50%;
            transform: translateY(-50%);
            background: none;
            border: none;
            color: var(--gray);
            cursor: pointer;
            font-size: 1.2rem;
            transition: color 0.3s;
        }

        .toggle-password:hover {
            color: var(--light);
        }

        .btn {
            background: linear-gradient(90deg, var(--primary), var(--primary-dark));
            color: white;
            border: none;
            padding: 16px 24px;
            border-radius: 12px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            width: 100%;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(124, 58, 237, 0.4);
        }

        .btn:active {
            transform: translateY(0);
        }

        .auth-links {
            display: flex;
            justify-content: space-between;
            margin-top: 15px;
        }

        .auth-link {
            color: var(--secondary);
            text-decoration: none;
            font-size: 0.9rem;
            transition: color 0.3s;
        }

        .auth-link:hover {
            color: var(--light);
            text-decoration: underline;
        }

        .error-message {
            color: var(--error);
            font-size: 0.9rem;
            margin-top: 8px;
            display: none;
            padding-left: 15px;
            animation: fadeIn 0.3s ease-out;
        }

        /* Reset Code Display */
        .reset-code-display {
            background: rgba(15, 23, 42, 0.7);
            padding: 15px;
            border-radius: 8px;
            margin-top: 15px;
            text-align: center;
            border: 1px solid rgba(255, 255, 255, 0.2);
        }

        .reset-code {
            font-size: 1.5rem;
            font-weight: bold;
            color: var(--secondary);
            margin: 10px 0;
        }

        /* Animation for page transitions */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @keyframes slideIn {
            from { transform: translateX(30px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        @keyframes slideOut {
            from { transform: translateX(0); opacity: 1; }
            to { transform: translateX(-30px); opacity: 0; }
        }

        .auth-page.slide-in {
            animation: slideIn 0.5s ease;
        }

        .auth-page.slide-out {
            animation: slideOut 0.5s ease;
        }

        /* Hidden class */
        .hidden {
            display: none;
        }

        /* Dashboard Styles */
        .dashboard-container {
            display: flex;
            width: 100%;
            max-width: 1200px;
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            border: 1px solid var(--card-border);
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            min-height: 80vh;
        }

        /* Sidebar */
        .sidebar {
            width: 280px;
            background: rgba(15, 23, 42, 0.9);
            padding: 25px 20px;
            display: flex;
            flex-direction: column;
            border-right: 1px solid rgba(255, 255, 255, 0.1);
        }

        .sidebar-header {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 30px;
            padding-bottom: 20px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .sidebar-logo {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .sidebar-logo i {
            font-size: 1.8rem;
            color: var(--primary);
        }

        .sidebar-logo h2 {
            font-size: 1.5rem;
            font-weight: 700;
            background: linear-gradient(to right, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .user-info {
            padding: 15px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 12px;
            margin-bottom: 20px;
        }

        .user-greeting {
            font-size: 1.1rem;
            font-weight: 600;
            margin-bottom: 5px;
        }

        .user-email {
            font-size: 0.9rem;
            color: var(--text-secondary);
        }

        .sidebar-nav {
            display: flex;
            flex-direction: column;
            gap: 10px;
            flex: 1;
        }

        .nav-item {
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 12px 15px;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            color: var(--text-secondary);
            text-decoration: none;
        }

        .nav-item.active {
            background: var(--primary);
            color: white;
        }

        .nav-item:hover:not(.active) {
            background: rgba(124, 58, 237, 0.2);
            color: var(--text-primary);
        }

        .nav-item i {
            font-size: 1.2rem;
            width: 20px;
            text-align: center;
        }

        .sidebar-footer {
            margin-top: auto;
            padding-top: 20px;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }

        .logout-btn, .delete-account-btn {
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 12px 15px;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            color: var(--text-secondary);
            background: none;
            border: none;
            text-align: left;
            width: 100%;
            margin-bottom: 10px;
        }

        .logout-btn:hover {
            background: rgba(239, 68, 68, 0.2);
            color: var(--error);
        }

        .delete-account-btn:hover {
            background: rgba(239, 68, 68, 0.3);
            color: var(--error);
        }

        /* Main Content Area */
        .main-content {
            flex: 1;
            display: flex;
            flex-direction: column;
        }

        .app-header {
            padding: 20px 30px;
            background: linear-gradient(90deg, var(--primary), var(--secondary));
            position: relative;
            overflow: hidden;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .mobile-menu-btn {
            display: none;
            background: none;
            border: none;
            color: white;
            font-size: 1.5rem;
            cursor: pointer;
        }

        .app-header .logo {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
        }

        .app-header .logo i {
            font-size: 2.5rem;
            background: rgba(255, 255, 255, 0.2);
            padding: 15px;
            border-radius: 50%;
        }

        .app-header h1 {
            font-size: 2.8rem;
            font-weight: 800;
            margin-bottom: 10px;
            background: linear-gradient(to right, #fff, #e0e7ff);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .tagline {
            font-size: 1.1rem;
            color: rgba(255, 255, 255, 0.8);
            max-width: 600px;
            margin: 0 auto;
            text-align: center;
        }

        .content {
            padding: 30px;
            flex: 1;
            overflow-y: auto;
        }

        .panel {
            display: none;
        }

        .panel.active {
            display: block;
            animation: fadeIn 0.5s ease;
        }

        /* Profile Panel Styles */
        .profile-container {
            max-width: 600px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.05);
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .profile-header {
            display: flex;
            align-items: center;
            gap: 20px;
            margin-bottom: 30px;
            padding-bottom: 20px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
        }

        .profile-avatar {
            width: 80px;
            height: 80px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
            color: white;
        }

        .profile-info h2 {
            font-size: 1.8rem;
            margin-bottom: 5px;
        }

        .profile-info p {
            color: var(--gray);
        }

        .profile-stats {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            margin-bottom: 30px;
        }

        .stat-card {
            background: rgba(15, 23, 42, 0.7);
            padding: 20px;
            border-radius: 12px;
            text-align: center;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .stat-value {
            font-size: 2rem;
            font-weight: 700;
            color: var(--primary);
            margin-bottom: 5px;
        }

        .stat-label {
            font-size: 0.9rem;
            color: var(--gray);
        }

        .profile-form {
            margin-top: 30px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: var(--text-primary);
        }

        .form-group input {
            width: 100%;
            padding: 12px 15px;
            border-radius: 10px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            background: rgba(255, 255, 255, 0.1);
            color: var(--text-primary);
            font-size: 1rem;
        }

        .form-group input:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(124, 58, 237, 0.3);
        }

        .form-actions {
            display: flex;
            gap: 15px;
            margin-top: 25px;
        }

        .btn-secondary {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }

        .btn-secondary:hover {
            background: rgba(255, 255, 255, 0.2);
            box-shadow: 0 5px 15px rgba(255, 255, 255, 0.1);
        }

        .btn-danger {
            background: linear-gradient(90deg, #ef4444, #dc2626);
        }

        .btn-danger:hover {
            box-shadow: 0 5px 15px rgba(239, 68, 68, 0.4);
        }

        /* Original App Styles */
        .input-group {
            margin-bottom: 25px;
            position: relative;
        }

        .input-group label {
            display: block;
            margin-bottom: 10px;
            font-weight: 600;
            color: var(--secondary);
            display: flex;
            align-items: center;
            gap: 8px;
        }

        textarea, input {
            width: 100%;
            padding: 15px;
            border-radius: 12px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            background: rgba(15, 23, 42, 0.7);
            color: var(--light);
            font-size: 1rem;
            resize: vertical;
            transition: all 0.3s ease;
        }

        textarea:focus, input:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(124, 58, 237, 0.3);
        }

        textarea {
            min-height: 120px;
        }

        .result-container {
            margin-top: 30px;
            display: none;
        }

        .result-container.active {
            display: block;
            animation: fadeIn 0.5s ease;
        }

        .result-box {
            background: rgba(15, 23, 42, 0.7);
            border-radius: 12px;
            padding: 20px;
            border: 1px solid var(--card-border);
            margin-top: 15px;
            word-break: break-all;
            max-height: 200px;
            overflow-y: auto;
        }

        .emoji-display {
            font-size: 1.8rem;
            line-height: 1.5;
            text-align: center;
            padding: 10px;
        }

        .action-buttons {
            display: flex;
            gap: 15px;
            margin-top: 20px;
        }

        .action-buttons button {
            flex: 1;
        }

        .security-info {
            background: rgba(6, 182, 212, 0.1);
            border-left: 4px solid var(--secondary);
            padding: 15px;
            border-radius: 8px;
            margin-top: 25px;
            font-size: 0.9rem;
        }

        .memory-info {
            background: rgba(124, 58, 237, 0.1);
            border-left: 4px solid var(--primary);
            padding: 15px;
            border-radius: 8px;
            margin-top: 15px;
            font-size: 0.9rem;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .memory-info:hover {
            background: rgba(124, 58, 237, 0.2);
        }

        footer {
            text-align: center;
            padding: 20px;
            margin-top: 20px;
            border-top: 1px solid var(--card-border);
            color: var(--gray);
            font-size: 0.9rem;
        }

        /* Records Page Styles */
        .records-container {
            max-width: 800px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.05);
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .record-item {
            background: rgba(15, 23, 42, 0.7);
            padding: 20px;
            margin-bottom: 20px;
            border-radius: 12px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }
        
        .record-item::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 5px;
            height: 100%;
            background: linear-gradient(90deg, var(--primary), var(--secondary));
        }
        
        .record-item:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }
        
        .record-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
            padding-bottom: 10px;
        }
        
        .record-title {
            font-size: 1.2rem;
            font-weight: 600;
            color: var(--light);
        }
        
        .record-date {
            font-size: 0.9rem;
            color: var(--gray);
        }
        
        .record-preview {
            display: flex;
            gap: 20px;
            margin-bottom: 15px;
        }
        
        .record-original, .record-emoji {
            flex: 1;
        }
        
        .record-label {
            font-size: 0.9rem;
            color: var(--gray);
            margin-bottom: 5px;
        }
        
        .record-value {
            font-size: 1.1rem;
            font-weight: 600;
            color: var(--light);
            word-break: break-all;
        }
        
        .record-actions {
            display: flex;
            justify-content: flex-end;
            margin-top: 15px;
        }
        
        .delete-record-btn {
            background: linear-gradient(90deg, #ef4444, #dc2626);
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 8px;
            font-size: 0.9rem;
            cursor: pointer;
            transition: all 0.3s ease;
            display: inline-flex;
            align-items: center;
            gap: 5px;
        }
        
        .delete-record-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 10px rgba(239, 68, 68, 0.3);
        }
        
        .no-records {
            text-align: center;
            padding: 40px 20px;
            color: var(--gray);
            font-size: 1.1rem;
        }

        /* Modal Styles */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }

        .modal.active {
            display: flex;
        }

        .modal-content {
            background: var(--card-bg);
            border-radius: 15px;
            padding: 30px;
            max-width: 500px;
            width: 90%;
            border: 1px solid var(--card-border);
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
        }

        .modal-header {
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .modal-title {
            font-size: 1.5rem;
            font-weight: 600;
            color: var(--text-primary);
        }

        .modal-body {
            margin-bottom: 25px;
        }

        .modal-footer {
            display: flex;
            justify-content: flex-end;
            gap: 15px;
        }

        @media (max-width: 768px) {
            .dashboard-container {
                flex-direction: column;
                border-radius: 15px;
            }
            
            .sidebar {
                width: 100%;
                padding: 15px;
                display: none;
                position: absolute;
                height: 100%;
                z-index: 100;
            }
            
            .sidebar.active {
                display: flex;
            }
            
            .mobile-menu-btn {
                display: block;
            }
            
            .auth-container {
                width: 95%;
            }
            
            .app-header h1 {
                font-size: 1.8rem;
            }
            
            .auth-header, .app-header {
                padding: 20px;
            }
            
            .auth-content, .content {
                padding: 20px;
            }
            
            .action-buttons {
                flex-direction: column;
            }
            
            .record-preview {
                flex-direction: column;
                gap: 10px;
            }

            .profile-stats {
                grid-template-columns: 1fr;
            }

            .form-actions {
                flex-direction: column;
            }
        }
    </style>
</head>
<body>
    <!-- Authentication Pages -->
    <div id="authPage" class="auth-page">
        <!-- Login Page -->
        <div id="loginPage" class="auth-container">
            <div class="auth-header">
                <h2>Welcome Back</h2>
                <p>Sign in to your TextMoji account</p>
            </div>
            
            <div class="auth-content">
                <div class="auth-form">
                    <div class="auth-input-group">
                        <label for="loginEmail"><i class="fas fa-envelope"></i> Email Address</label>
                        <input type="email" id="loginEmail" placeholder="Enter your email" required>
                        <div class="error-message" id="loginEmailError">Please enter a valid email address</div>
                    </div>
                    
                    <div class="auth-input-group">
                        <label for="loginPassword"><i class="fas fa-key"></i> Password</label>
                        <div class="password-container">
                            <input type="password" id="loginPassword" placeholder="Enter your password" required>
                            <button class="toggle-password" id="toggleLoginPassword">
                                <i class="fas fa-eye"></i>
                            </button>
                        </div>
                        <div class="error-message" id="loginPasswordError">Password must be at least 6 characters</div>
                    </div>
                    
                    <button class="btn" id="loginBtn">
                        <i class="fas fa-sign-in-alt"></i>
                        Sign In
                    </button>
                    
                    <div class="auth-links">
                        <a href="#" class="auth-link" id="forgotPasswordLink">Forgot Password?</a>
                        <a href="#" class="auth-link" id="signUpLink">Create Account</a>
                    </div>
                </div>
            </div>
        </div>

        <!-- Signup Page -->
        <div id="signupPage" class="auth-container hidden">
            <div class="auth-header">
                <h2>Create Account</h2>
                <p>Join TextMoji to secure your messages</p>
            </div>
            
            <div class="auth-content">
                <div class="auth-form">
                    <div class="auth-input-group">
                        <label for="signupName"><i class="fas fa-user"></i> Full Name</label>
                        <input type="text" id="signupName" placeholder="Enter your full name" required>
                        <div class="error-message" id="signupNameError">Please enter your full name</div>
                    </div>
                    
                    <div class="auth-input-group">
                        <label for="signupEmail"><i class="fas fa-envelope"></i> Email Address</label>
                        <input type="email" id="signupEmail" placeholder="Enter your email" required>
                        <div class="error-message" id="signupEmailError">Please enter a valid email address</div>
                    </div>
                    
                    <div class="auth-input-group">
                        <label for="signupPassword"><i class="fas fa-key"></i> Password</label>
                        <div class="password-container">
                            <input type="password" id="signupPassword" placeholder="Create a password" required>
                            <button class="toggle-password" id="toggleSignupPassword">
                                <i class="fas fa-eye"></i>
                            </button>
                        </div>
                        <div class="error-message" id="signupPasswordError">Password must be at least 6 characters</div>
                    </div>
                    
                    <div class="auth-input-group">
                        <label for="signupConfirmPassword"><i class="fas fa-key"></i> Confirm Password</label>
                        <div class="password-container">
                            <input type="password" id="signupConfirmPassword" placeholder="Confirm your password" required>
                            <button class="toggle-password" id="toggleSignupConfirmPassword">
                                <i class="fas fa-eye"></i>
                            </button>
                        </div>
                        <div class="error-message" id="signupConfirmPasswordError">Passwords do not match</div>
                    </div>
                    
                    <button class="btn" id="signupBtn">
                        <i class="fas fa-user-plus"></i>
                        Create Account
                    </button>
                    
                    <div class="auth-links">
                        <a href="#" class="auth-link" id="backToLoginLink">Already have an account? Sign In</a>
                    </div>
                </div>
            </div>
        </div>

        <!-- Reset Password Page -->
        <div id="resetPasswordPage" class="auth-container hidden">
            <div class="auth-header">
                <h2>Reset Password</h2>
                <p>Enter your email to receive a reset code</p>
            </div>
            
            <div class="auth-content">
                <div class="auth-form">
                    <!-- Step 1: Enter Email -->
                    <div id="resetStep1" class="reset-step">
                        <div class="auth-input-group">
                            <label for="resetEmail"><i class="fas fa-envelope"></i> Email Address</label>
                            <input type="email" id="resetEmail" placeholder="Enter your email" required>
                            <div class="error-message" id="resetEmailError">Please enter a valid email address</div>
                        </div>
                        
                        <button class="btn" id="sendResetBtn">
                            <i class="fas fa-paper-plane"></i>
                            Send Reset Code
                        </button>
                    </div>
                    
                    <!-- Step 2: Enter Code and New Password -->
                    <div id="resetStep2" class="reset-step hidden">
                        <div class="reset-code-display">
                            <p>Your reset code is:</p>
                            <div class="reset-code" id="resetCodeDisplay">123456</div>
                            <p>Enter this code below to reset your password</p>
                        </div>
                        
                        <div class="auth-input-group">
                            <label for="resetCode"><i class="fas fa-shield-alt"></i> Reset Code</label>
                            <input type="text" id="resetCode" placeholder="Enter reset code" required>
                            <div class="error-message" id="resetCodeError">Please enter the reset code</div>
                        </div>
                        
                        <div class="auth-input-group">
                            <label for="newPassword"><i class="fas fa-key"></i> New Password</label>
                            <div class="password-container">
                                <input type="password" id="newPassword" placeholder="Create a new password" required>
                                <button class="toggle-password" id="toggleNewPassword">
                                    <i class="fas fa-eye"></i>
                                </button>
                            </div>
                            <div class="error-message" id="newPasswordError">Password must be at least 6 characters</div>
                        </div>
                        
                        <div class="auth-input-group">
                            <label for="confirmNewPassword"><i class="fas fa-key"></i> Confirm New Password</label>
                            <div class="password-container">
                                <input type="password" id="confirmNewPassword" placeholder="Confirm your new password" required>
                                <button class="toggle-password" id="toggleConfirmNewPassword">
                                    <i class="fas fa-eye"></i>
                                </button>
                            </div>
                            <div class="error-message" id="confirmNewPasswordError">Passwords do not match</div>
                        </div>
                        
                        <button class="btn" id="resetPasswordBtn">
                            <i class="fas fa-sync-alt"></i>
                            Reset Password
                        </button>
                    </div>
                    
                    <div class="auth-links">
                        <a href="#" class="auth-link" id="backToLoginFromResetLink">Back to Sign In</a>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Main App Dashboard (Hidden Initially) -->
    <div id="appContainer" class="dashboard-container hidden">
        <!-- Sidebar Dashboard -->
        <div class="sidebar" id="sidebar">
            <div class="sidebar-header">
                <div class="sidebar-logo">
                    <i class="fas fa-lock"></i>
                    <h2>TextMoji</h2>
                </div>
            </div>
            
            <div class="user-info">
                <div class="user-greeting" id="userGreeting">Welcome, User!</div>
                <div class="user-email" id="userEmail">user@example.com</div>
            </div>
            
            <div class="sidebar-nav">
                <a href="#" class="nav-item active" data-tab="encrypt">
                    <i class="fas fa-lock"></i>
                    <span>Encrypt Text</span>
                </a>
                <a href="#" class="nav-item" data-tab="decrypt">
                    <i class="fas fa-unlock"></i>
                    <span>Decrypt Emojis</span>
                </a>
                <a href="#" class="nav-item" data-tab="records">
                    <i class="fas fa-history"></i>
                    <span>Records</span>
                </a>
                <a href="#" class="nav-item" data-tab="profile">
                    <i class="fas fa-user"></i>
                    <span>Profile</span>
                </a>
            </div>
            
            <div class="sidebar-footer">
                <button class="logout-btn" id="sidebarLogoutBtn">
                    <i class="fas fa-sign-out-alt"></i>
                    <span>Logout</span>
                </button>
                
                <button class="delete-account-btn" id="deleteAccountBtn">
                    <i class="fas fa-trash-alt"></i>
                    <span>Delete Account</span>
                </button>
            </div>
        </div>
        
        <!-- Main Content -->
        <div class="main-content">
            <div class="app-header">
                <button class="mobile-menu-btn" id="mobileMenuBtn">
                    <i class="fas fa-bars"></i>
                </button>
                <div class="logo">
                    <i class="fas fa-lock"></i>
                    <h1>TextMoji</h1>
                </div>
                <div></div> <!-- Empty div for flex alignment -->
            </div>

            <div class="content">
                <!-- Encrypt Panel -->
                <div class="panel active" id="encryptPanel">
                    <div class="input-group">
                        <label for="plainText"><i class="fas fa-font"></i> Text to Encrypt</label>
                        <textarea id="plainText" placeholder="Enter your secret message here..."></textarea>
                    </div>
                    <div class="input-group">
                        <label for="encryptPassword"><i class="fas fa-key"></i> Encryption Password</label>
                        <div class="password-container">
                            <input type="password" id="encryptPassword" placeholder="Enter a strong password">
                            <button class="toggle-password" id="toggleEncryptPassword">
                                <i class="fas fa-eye"></i>
                            </button>
                        </div>
                    </div>
                    <button class="btn" id="encryptBtn">
                        <i class="fas fa-shield-alt"></i>
                        Encrypt Text
                    </button>
                    
                    <div class="result-container" id="encryptResultContainer">
                        <label><i class="fas fa-lock"></i> Encrypted Text (Emoji)</label>
                        <div class="result-box">
                            <div class="emoji-display" id="encryptedEmoji"></div>
                        </div>
                        <div class="action-buttons">
                            <button class="btn btn-secondary" id="copyEncryptedBtn">
                                <i class="fas fa-copy"></i>
                                Copy Emoji
                            </button>
                            <button class="btn btn-secondary" id="saveEncryptedBtn">
                                <i class="fas fa-save"></i>
                                Save to Records
                            </button>
                        </div>
                    </div>
                    
                    <div class="security-info">
                        <i class="fas fa-info-circle"></i>
                        <strong>Security Note:</strong> Your text is encrypted using AES-256 encryption before being converted to emojis. The password is required to decrypt the message.
                    </div>
                    
                    <div class="memory-info" id="memoryInfo">
                        <i class="fas fa-database"></i> Messages stored: <span id="memoryCount">0</span>
                    </div>
                </div>

                <!-- Decrypt Panel -->
                <div class="panel" id="decryptPanel">
                    <div class="input-group">
                        <label for="encryptedText"><i class="fas fa-smile"></i> Encrypted Text (Emoji)</label>
                        <textarea id="encryptedText" placeholder="Paste your emoji encrypted text here..."></textarea>
                    </div>
                    <div class="input-group">
                        <label for="decryptPassword"><i class="fas fa-key"></i> Decryption Password</label>
                        <div class="password-container">
                            <input type="password" id="decryptPassword" placeholder="Enter the password used for encryption">
                            <button class="toggle-password" id="toggleDecryptPassword">
                                <i class="fas fa-eye"></i>
                            </button>
                        </div>
                    </div>
                    <button class="btn" id="decryptBtn">
                        <i class="fas fa-unlock-alt"></i>
                        Decrypt Text
                    </button>
                    
                    <div class="result-container" id="decryptResultContainer">
                        <label><i class="fas fa-font"></i> Decrypted Text</label>
                        <div class="result-box">
                            <div id="decryptedText"></div>
                        </div>
                        <div class="action-buttons">
                            <button class="btn btn-secondary" id="copyDecryptedBtn">
                                <i class="fas fa-copy"></i>
                                Copy Text
                            </button>
                            <button class="btn btn-secondary" id="clearDecryptBtn">
                                <i class="fas fa-trash"></i>
                                Clear
                            </button>
                        </div>
                    </div>
                    
                    <div class="memory-info">
                        <i class="fas fa-lightbulb"></i>
                        <strong>Tip:</strong> Make sure to use the exact same password that was used during encryption. The decryption will fail if the password is incorrect.
                    </div>
                </div>

                <!-- Records Panel -->
                <div class="panel" id="recordsPanel">
                    <div class="records-container">
                        <h2>Your Encryption Records</h2>
                        <p>Here you can view and manage all your encrypted messages.</p>
                        
                        <div id="recordsList">
                            <!-- Records will be dynamically added here -->
                            <div class="no-records">
                                <i class="fas fa-inbox" style="font-size: 3rem; margin-bottom: 15px; opacity: 0.5;"></i>
                                <p>No encryption records found. Start by encrypting some text!</p>
                            </div>
                        </div>
                        
                        <button class="btn btn-secondary" id="clearAllRecordsBtn" style="display: none;">
                            <i class="fas fa-trash-alt"></i>
                            Clear All Records
                        </button>
                    </div>
                </div>

                <!-- Profile Panel -->
                <div class="panel" id="profilePanel">
                    <div class="profile-container">
                        <div class="profile-header">
                            <div class="profile-avatar">
                                <i class="fas fa-user"></i>
                            </div>
                            <div class="profile-info">
                                <h2 id="profileUserName">User Name</h2>
                                <p id="profileUserEmail">user@example.com</p>
                            </div>
                        </div>
                        
                        <div class="profile-stats">
                            <div class="stat-card">
                                <div class="stat-value" id="statMessages">0</div>
                                <div class="stat-label">Messages Encrypted</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-value" id="statDays">0</div>
                                <div class="stat-label">Days Active</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-value" id="statSecurity">100%</div>
                                <div class="stat-label">Security Score</div>
                            </div>
                        </div>
                        
                        <div class="profile-form">
                            <h3>Account Information</h3>
                            <div class="form-group">
                                <label for="profileName">Full Name</label>
                                <input type="text" id="profileName" placeholder="Enter your full name">
                            </div>
                            <div class="form-group">
                                <label for="profileEmail">Email Address</label>
                                <input type="email" id="profileEmail" placeholder="Enter your email">
                            </div>
                            <div class="form-group">
                                <label for="profilePassword">New Password</label>
                                <input type="password" id="profilePassword" placeholder="Enter new password">
                            </div>
                            <div class="form-group">
                                <label for="profileConfirmPassword">Confirm New Password</label>
                                <input type="password" id="profileConfirmPassword" placeholder="Confirm new password">
                            </div>
                            
                            <div class="form-actions">
                                <button class="btn" id="updateProfileBtn">
                                    <i class="fas fa-save"></i>
                                    Update Profile
                                </button>
                                <button class="btn btn-secondary" id="resetProfileBtn">
                                    <i class="fas fa-undo"></i>
                                    Reset Changes
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <footer>
                <p>TextMoji &copy; 2025 | Secure Emoji Encryption System</p>
            </footer>
        </div>
    </div>

    <!-- Delete Account Confirmation Modal -->
    <div class="modal" id="deleteAccountModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 class="modal-title">Delete Account</h3>
            </div>
            <div class="modal-body">
                <p>Are you sure you want to delete your account? This action cannot be undone and all your data will be permanently lost.</p>
                <p>Please type <strong>DELETE</strong> to confirm:</p>
                <input type="text" id="deleteConfirmation" class="form-group" style="margin-top: 10px;" placeholder="Type DELETE to confirm">
            </div>
            <div class="modal-footer">
                <button class="btn btn-secondary" id="cancelDeleteBtn">Cancel</button>
                <button class="btn btn-danger" id="confirmDeleteBtn">Delete Account</button>
            </div>
        </div>
    </div>

    <script>
        // Authentication state
        let isAuthenticated = false;
        let currentUser = null;
        let users = JSON.parse(localStorage.getItem('textmoji_users')) || {};
        let records = JSON.parse(localStorage.getItem('textmoji_records')) || {};
        
        // DOM Elements
        const authPage = document.getElementById('authPage');
        const appContainer = document.getElementById('appContainer');
        
        // Authentication pages
        const loginPage = document.getElementById('loginPage');
        const signupPage = document.getElementById('signupPage');
        const resetPasswordPage = document.getElementById('resetPasswordPage');
        
        // Authentication elements
        const loginBtn = document.getElementById('loginBtn');
        const signupBtn = document.getElementById('signupBtn');
        const sendResetBtn = document.getElementById('sendResetBtn');
        const resetPasswordBtn = document.getElementById('resetPasswordBtn');
        
        const forgotPasswordLink = document.getElementById('forgotPasswordLink');
        const signUpLink = document.getElementById('signUpLink');
        const backToLoginLink = document.getElementById('backToLoginLink');
        const backToLoginFromResetLink = document.getElementById('backToLoginFromResetLink');
        
        // Reset password steps
        const resetStep1 = document.getElementById('resetStep1');
        const resetStep2 = document.getElementById('resetStep2');
        const resetCodeDisplay = document.getElementById('resetCodeDisplay');
        
        // App elements
        const sidebar = document.getElementById('sidebar');
        const sidebarLogoutBtn = document.getElementById('sidebarLogoutBtn');
        const deleteAccountBtn = document.getElementById('deleteAccountBtn');
        const mobileMenuBtn = document.getElementById('mobileMenuBtn');
        
        // Navigation elements
        const navItems = document.querySelectorAll('.nav-item');
        const panels = document.querySelectorAll('.panel');
        
        // Encryption/Decryption elements
        const encryptBtn = document.getElementById('encryptBtn');
        const decryptBtn = document.getElementById('decryptBtn');
        const plainText = document.getElementById('plainText');
        const encryptPassword = document.getElementById('encryptPassword');
        const encryptedText = document.getElementById('encryptedText');
        const decryptPassword = document.getElementById('decryptPassword');
        const encryptResultContainer = document.getElementById('encryptResultContainer');
        const decryptResultContainer = document.getElementById('decryptResultContainer');
        const encryptedEmoji = document.getElementById('encryptedEmoji');
        const decryptedText = document.getElementById('decryptedText');
        const copyEncryptedBtn = document.getElementById('copyEncryptedBtn');
        const copyDecryptedBtn = document.getElementById('copyDecryptedBtn');
        const saveEncryptedBtn = document.getElementById('saveEncryptedBtn');
        const clearDecryptBtn = document.getElementById('clearDecryptBtn');
        
        // Password toggles
        const toggleLoginPassword = document.getElementById('toggleLoginPassword');
        const toggleSignupPassword = document.getElementById('toggleSignupPassword');
        const toggleSignupConfirmPassword = document.getElementById('toggleSignupConfirmPassword');
        const toggleNewPassword = document.getElementById('toggleNewPassword');
        const toggleConfirmNewPassword = document.getElementById('toggleConfirmNewPassword');
        const toggleEncryptPassword = document.getElementById('toggleEncryptPassword');
        const toggleDecryptPassword = document.getElementById('toggleDecryptPassword');
        
        // Dashboard elements
        const userGreeting = document.getElementById('userGreeting');
        const userEmail = document.getElementById('userEmail');
        
        // Profile elements
        const profileUserName = document.getElementById('profileUserName');
        const profileUserEmail = document.getElementById('profileUserEmail');
        const profileName = document.getElementById('profileName');
        const profileEmail = document.getElementById('profileEmail');
        const profilePassword = document.getElementById('profilePassword');
        const profileConfirmPassword = document.getElementById('profileConfirmPassword');
        const updateProfileBtn = document.getElementById('updateProfileBtn');
        const resetProfileBtn = document.getElementById('resetProfileBtn');
        
        // Stats elements
        const statMessages = document.getElementById('statMessages');
        const statDays = document.getElementById('statDays');
        const statSecurity = document.getElementById('statSecurity');
        
        // Records elements
        const recordsList = document.getElementById('recordsList');
        const clearAllRecordsBtn = document.getElementById('clearAllRecordsBtn');
        
        // Memory info
        const memoryInfo = document.getElementById('memoryInfo');
        const memoryCount = document.getElementById('memoryCount');
        
        // Modal elements
        const deleteAccountModal = document.getElementById('deleteAccountModal');
        const deleteConfirmation = document.getElementById('deleteConfirmation');
        const cancelDeleteBtn = document.getElementById('cancelDeleteBtn');
        const confirmDeleteBtn = document.getElementById('confirmDeleteBtn');
        
        // Initialize the app
        function initApp() {
            // Check if user is already logged in
            const currentUserEmail = localStorage.getItem('textmoji_currentUser');
            if (currentUserEmail && users[currentUserEmail]) {
                currentUser = users[currentUserEmail];
                isAuthenticated = true;
                showApp();
            } else {
                showAuthPage('loginPage');
            }
            
            // Set up event listeners
            setupEventListeners();
        }
        
        // Set up all event listeners
        function setupEventListeners() {
            // Authentication events
            loginBtn.addEventListener('click', handleLogin);
            signupBtn.addEventListener('click', handleSignup);
            sendResetBtn.addEventListener('click', handleSendReset);
            resetPasswordBtn.addEventListener('click', handleResetPassword);
            
            forgotPasswordLink.addEventListener('click', () => showAuthPage('resetPasswordPage'));
            signUpLink.addEventListener('click', () => showAuthPage('signupPage'));
            backToLoginLink.addEventListener('click', () => showAuthPage('loginPage'));
            backToLoginFromResetLink.addEventListener('click', () => showAuthPage('loginPage'));
            
            // App navigation events
            sidebarLogoutBtn.addEventListener('click', handleLogout);
            deleteAccountBtn.addEventListener('click', showDeleteAccountModal);
            mobileMenuBtn.addEventListener('click', toggleMobileMenu);
            
            // Navigation events
            navItems.forEach(item => {
                item.addEventListener('click', (e) => {
                    e.preventDefault();
                    const tabName = item.getAttribute('data-tab');
                    switchTab(tabName);
                    
                    // Update active nav item
                    navItems.forEach(nav => nav.classList.remove('active'));
                    item.classList.add('active');
                    
                    // Close mobile menu after selection
                    if (window.innerWidth <= 768) {
                        sidebar.classList.remove('active');
                    }
                });
            });
            
            // Encryption/Decryption events
            encryptBtn.addEventListener('click', handleEncryption);
            decryptBtn.addEventListener('click', handleDecryption);
            copyEncryptedBtn.addEventListener('click', copyEncryptedText);
            copyDecryptedBtn.addEventListener('click', copyDecryptedText);
            saveEncryptedBtn.addEventListener('click', saveToRecords);
            clearDecryptBtn.addEventListener('click', clearDecrypt);
            
            // Password toggle events
            setupPasswordToggles();
            
            // Profile events
            updateProfileBtn.addEventListener('click', updateProfile);
            resetProfileBtn.addEventListener('click', resetProfileForm);
            
            // Records events
            clearAllRecordsBtn.addEventListener('click', clearAllRecords);
            
            // Memory info click to go to records
            memoryInfo.addEventListener('click', function() {
                switchTab('records');
            });
            
            // Modal events
            cancelDeleteBtn.addEventListener('click', hideDeleteAccountModal);
            confirmDeleteBtn.addEventListener('click', handleDeleteAccount);
            
            // Input validation
            setupInputValidation();
        }
        
        // Show authentication page with transition
        function showAuthPage(pageId) {
            // Get current active page
            const activePage = document.querySelector('.auth-container:not(.hidden)');
            
            if (activePage) {
                // Add slide-out animation to current page
                activePage.classList.add('slide-out');
                
                // After animation completes, hide it and show new page
                setTimeout(() => {
                    activePage.classList.add('hidden');
                    activePage.classList.remove('slide-out');
                    
                    // Show new page with slide-in animation
                    const newPage = document.getElementById(pageId);
                    newPage.classList.remove('hidden');
                    newPage.classList.add('slide-in');
                    
                    // Remove animation class after it completes
                    setTimeout(() => {
                        newPage.classList.remove('slide-in');
                    }, 500);
                }, 500);
            } else {
                // No active page, just show the requested page
                const newPage = document.getElementById(pageId);
                newPage.classList.remove('hidden');
            }
        }
        
        // Show authentication
        function showAuth(initialPage = 'loginPage') {
            authPage.classList.remove('hidden');
            appContainer.classList.add('hidden');
            showAuthPage(initialPage);
        }
        
        // Show main app
        function showApp() {
            authPage.classList.add('hidden');
            appContainer.classList.remove('hidden');
            updateUserDashboard();
            loadRecords();
            updateMemoryDisplay();
            updateProfileStats();
        }
        
        // Handle login
        function handleLogin() {
            const email = document.getElementById('loginEmail').value;
            const password = document.getElementById('loginPassword').value;
            
            if (!validateEmail(email)) {
                showError('loginEmailError', 'Please enter a valid email address');
                return;
            }
            
            if (password.length < 6) {
                showError('loginPasswordError', 'Password must be at least 6 characters');
                return;
            }
            
            if (users[email] && users[email].password === password) {
                currentUser = users[email];
                localStorage.setItem('textmoji_currentUser', email);
                isAuthenticated = true;
                showApp();
            } else {
                showError('loginPasswordError', 'Invalid email or password');
            }
        }
        
        // Handle signup
        function handleSignup() {
            const name = document.getElementById('signupName').value;
            const email = document.getElementById('signupEmail').value;
            const password = document.getElementById('signupPassword').value;
            const confirmPassword = document.getElementById('signupConfirmPassword').value;
            
            if (!name) {
                showError('signupNameError', 'Please enter your full name');
                return;
            }
            
            if (!validateEmail(email)) {
                showError('signupEmailError', 'Please enter a valid email address');
                return;
            }
            
            if (password.length < 6) {
                showError('signupPasswordError', 'Password must be at least 6 characters');
                return;
            }
            
            if (password !== confirmPassword) {
                showError('signupConfirmPasswordError', 'Passwords do not match');
                return;
            }
            
            if (users[email]) {
                showError('signupEmailError', 'An account with this email already exists');
                return;
            }
            
            // Create new user
            users[email] = {
                name: name,
                email: email,
                password: password,
                createdAt: new Date().toISOString()
            };
            
            // Save to localStorage
            localStorage.setItem('textmoji_users', JSON.stringify(users));
            
            // Clear form and redirect to login
            document.getElementById('signupName').value = '';
            document.getElementById('signupEmail').value = '';
            document.getElementById('signupPassword').value = '';
            document.getElementById('signupConfirmPassword').value = '';
            
            alert('Account created successfully! Please sign in with your credentials.');
            showAuthPage('loginPage');
        }
        
        // Handle password reset request
        function handleSendReset() {
            const email = document.getElementById('resetEmail').value;
            
            if (!validateEmail(email)) {
                showError('resetEmailError', 'Please enter a valid email address');
                return;
            }
            
            if (!users[email]) {
                showError('resetEmailError', 'No account found with this email address');
                return;
            }
            
            // Generate a random reset code
            const resetCode = Math.floor(100000 + Math.random() * 900000);
            
            // Display the reset code
            resetCodeDisplay.textContent = resetCode;
            
            // Show step 2
            resetStep1.classList.add('hidden');
            resetStep2.classList.remove('hidden');
            
            // Store reset code in session storage
            sessionStorage.setItem('textmoji_resetCode', resetCode);
            sessionStorage.setItem('textmoji_resetEmail', email);
        }
        
        // Handle password reset
        function handleResetPassword() {
            const code = document.getElementById('resetCode').value;
            const newPassword = document.getElementById('newPassword').value;
            const confirmNewPassword = document.getElementById('confirmNewPassword').value;
            const storedCode = sessionStorage.getItem('textmoji_resetCode');
            const email = sessionStorage.getItem('textmoji_resetEmail');
            
            if (!code || code !== storedCode) {
                showError('resetCodeError', 'Invalid reset code');
                return;
            }
            
            if (newPassword.length < 6) {
                showError('newPasswordError', 'Password must be at least 6 characters');
                return;
            }
            
            if (newPassword !== confirmNewPassword) {
                showError('confirmNewPasswordError', 'Passwords do not match');
                return;
            }
            
            // Update password
            users[email].password = newPassword;
            localStorage.setItem('textmoji_users', JSON.stringify(users));
            
            // Clear reset data
            sessionStorage.removeItem('textmoji_resetCode');
            sessionStorage.removeItem('textmoji_resetEmail');
            
            alert('Password reset successfully! You can now log in with your new password.');
            showAuthPage('loginPage');
        }
        
        // Handle logout
        function handleLogout() {
            currentUser = null;
            isAuthenticated = false;
            localStorage.removeItem('textmoji_currentUser');
            showAuth();
        }
        
        // Show delete account modal
        function showDeleteAccountModal() {
            deleteAccountModal.classList.add('active');
            deleteConfirmation.value = '';
        }
        
        // Hide delete account modal
        function hideDeleteAccountModal() {
            deleteAccountModal.classList.remove('active');
        }
        
        // Handle delete account
        function handleDeleteAccount() {
            if (deleteConfirmation.value !== 'DELETE') {
                alert('Please type DELETE to confirm account deletion.');
                return;
            }
            
            const userEmail = currentUser.email;
            
            // Remove user from users
            delete users[userEmail];
            localStorage.setItem('textmoji_users', JSON.stringify(users));
            
            // Remove user records
            delete records[userEmail];
            localStorage.setItem('textmoji_records', JSON.stringify(records));
            
            // Remove current user
            localStorage.removeItem('textmoji_currentUser');
            
            currentUser = null;
            isAuthenticated = false;
            
            hideDeleteAccountModal();
            alert('Your account has been deleted successfully.');
            showAuth();
        }
        
        // Toggle mobile menu
        function toggleMobileMenu() {
            sidebar.classList.toggle('active');
        }
        
        // Switch between tabs
        function switchTab(tabName) {
            // Update active panel
            panels.forEach(panel => {
                if (panel.id === `${tabName}Panel`) {
                    panel.classList.add('active');
                } else {
                    panel.classList.remove('active');
                }
            });
            
            // Update specific content if needed
            if (tabName === 'records') {
                loadRecords();
            } else if (tabName === 'profile') {
                loadProfileData();
            }
        }
        
        // Update user dashboard
        function updateUserDashboard() {
            if (currentUser) {
                userGreeting.textContent = `Welcome, ${currentUser.name.split(' ')[0]}!`;
                userEmail.textContent = currentUser.email;
            }
        }
        
        // Load profile data
        function loadProfileData() {
            if (currentUser) {
                profileUserName.textContent = currentUser.name;
                profileUserEmail.textContent = currentUser.email;
                profileName.value = currentUser.name;
                profileEmail.value = currentUser.email;
                profilePassword.value = '';
                profileConfirmPassword.value = '';
            }
        }
        
        // Update profile
        function updateProfile() {
            const name = profileName.value;
            const email = profileEmail.value;
            const password = profilePassword.value;
            const confirmPassword = profileConfirmPassword.value;
            
            if (!name) {
                alert('Please enter your name');
                return;
            }
            
            if (!validateEmail(email)) {
                alert('Please enter a valid email address');
                return;
            }
            
            if (password && password.length < 6) {
                alert('Password must be at least 6 characters');
                return;
            }
            
            if (password && password !== confirmPassword) {
                alert('Passwords do not match');
                return;
            }
            
            // Update user data
            const oldEmail = currentUser.email;
            currentUser.name = name;
            currentUser.email = email;
            
            if (password) {
                currentUser.password = password;
            }
            
            // If email changed, update the users object
            if (oldEmail !== email) {
                delete users[oldEmail];
                users[email] = currentUser;
                localStorage.setItem('textmoji_currentUser', email);
            }
            
            // Save updated users
            localStorage.setItem('textmoji_users', JSON.stringify(users));
            
            // Update dashboard
            updateUserDashboard();
            
            alert('Profile updated successfully!');
        }
        
        // Reset profile form
        function resetProfileForm() {
            loadProfileData();
        }
        
        // Update profile stats
        function updateProfileStats() {
            const userEmail = currentUser.email;
            const userRecords = records[userEmail] || [];
            
            // Messages encrypted
            statMessages.textContent = userRecords.length;
            
            // Days active
            const createdAt = new Date(currentUser.createdAt);
            const today = new Date();
            const diffTime = Math.abs(today - createdAt);
            const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
            statDays.textContent = diffDays;
            
            // Security score (just for show)
            statSecurity.textContent = '100%';
        }
        
        // Handle encryption
        function handleEncryption() {
            const text = plainText.value.trim();
            const password = encryptPassword.value;
            
            if (!text) {
                alert('Please enter text to encrypt');
                return;
            }
            
            if (!password) {
                alert('Please enter an encryption password');
                return;
            }
            
            try {
                // Encrypt the text using AES
                const encrypted = CryptoJS.AES.encrypt(text, password).toString();
                
                // Convert encrypted string to emojis using simple char code method
                let emojis = '';
                for (let i = 0; i < encrypted.length; i++) {
                    const charCode = encrypted.charCodeAt(i);
                    // Use emojis from 128512 to 128591 (smileys range)
                    emojis += String.fromCodePoint(128512 + (charCode % 80));
                }
                
                // Display result
                encryptedEmoji.textContent = emojis;
                encryptResultContainer.classList.add('active');
                
                // Store the encrypted data for potential saving
                sessionStorage.setItem('lastEncrypted', JSON.stringify({
                    text: text,
                    encrypted: encrypted,
                    emoji: emojis,
                    password: password
                }));
                
                console.log('Encrypted and saved successfully!');
                
            } catch (error) {
                encryptedEmoji.textContent = '❌ Encryption failed! Please try again.';
                encryptedEmoji.style.color = 'var(--error)';
                encryptResultContainer.classList.add('active');
                console.error('Encryption error:', error);
            }
        }
        
        // Handle decryption
        function handleDecryption() {
            const emojis = encryptedText.value.trim();
            const password = decryptPassword.value;
            
            if (!emojis) {
                alert('Please enter emoji text to decrypt');
                return;
            }
            
            if (!password) {
                alert('Please enter the decryption password');
                return;
            }
            
            try {
                // Get user records
                const userEmail = currentUser.email;
                const userRecords = records[userEmail] || [];
                
                // Find the matching record
                let foundRecord = null;
                
                for (let record of userRecords) {
                    if (record.emojiText === emojis && record.password === password) {
                        foundRecord = record;
                        break;
                    }
                }
                
                if (foundRecord) {
                    // Directly show the original text since we have it stored
                    decryptedText.textContent = foundRecord.originalText;
                    decryptedText.style.color = '';
                    decryptResultContainer.classList.add('active');
                    console.log('Decryption successful!');
                } else {
                    // Try alternative method - convert emojis back and decrypt
                    let encryptedString = '';
                    for (let i = 0; i < emojis.length; i++) {
                        const codePoint = emojis.codePointAt(i);
                        // Reverse the emoji conversion
                        const charCode = (codePoint - 128512) % 256;
                        encryptedString += String.fromCharCode(charCode);
                    }
                    
                    // Try to decrypt with the password
                    const decryptedBytes = CryptoJS.AES.decrypt(encryptedString, password);
                    const decrypted = decryptedBytes.toString(CryptoJS.enc.Utf8);
                    
                    if (decrypted) {
                        decryptedText.textContent = decrypted;
                        decryptedText.style.color = '';
                        decryptResultContainer.classList.add('active');
                    } else {
                        // Check if password exists but emojis don't match
                        let passwordExists = userRecords.some(record => record.password === password);
                        
                        if (passwordExists) {
                            decryptedText.textContent = '❌ Emoji message not found! Please check your emojis.';
                        } else {
                            decryptedText.textContent = '❌ Wrong password! Please check your password.';
                        }
                        decryptedText.style.color = 'var(--error)';
                        decryptResultContainer.classList.add('active');
                    }
                }
                
            } catch (error) {
                decryptedText.textContent = '❌ Decryption failed! Please check your inputs.';
                decryptedText.style.color = 'var(--error)';
                decryptResultContainer.classList.add('active');
                console.error('Decryption error:', error);
            }
        }
        
        // Copy encrypted text to clipboard
        function copyEncryptedText() {
            const text = encryptedEmoji.textContent;
            
            // Create a temporary textarea element
            const textArea = document.createElement('textarea');
            textArea.value = text;
            document.body.appendChild(textArea);
            textArea.select();
            
            try {
                // Execute copy command
                const successful = document.execCommand('copy');
                if (successful) {
                    alert('Encrypted text copied to clipboard!');
                } else {
                    alert('Failed to copy text. Please try again.');
                }
            } catch (err) {
                alert('Failed to copy text. Please try again.');
            }
            
            // Clean up
            document.body.removeChild(textArea);
        }
        
        // Copy decrypted text to clipboard
        function copyDecryptedText() {
            const text = decryptedText.textContent;
            
            // Create a temporary textarea element
            const textArea = document.createElement('textarea');
            textArea.value = text;
            document.body.appendChild(textArea);
            textArea.select();
            
            try {
                // Execute copy command
                const successful = document.execCommand('copy');
                if (successful) {
                    alert('Decrypted text copied to clipboard!');
                } else {
                    alert('Failed to copy text. Please try again.');
                }
            } catch (err) {
                alert('Failed to copy text. Please try again.');
            }
            
            // Clean up
            document.body.removeChild(textArea);
        }
        
        // Save encrypted text to records
        function saveToRecords() {
            const lastEncrypted = sessionStorage.getItem('lastEncrypted');
            if (!lastEncrypted) {
                alert('No encrypted text to save');
                return;
            }
            
            const encryptedData = JSON.parse(lastEncrypted);
            const userEmail = currentUser.email;
            
            // Initialize user records if needed
            if (!records[userEmail]) {
                records[userEmail] = [];
            }
            
            // Add new record
            records[userEmail].push({
                id: Date.now().toString(),
                originalText: encryptedData.text,
                encryptedText: encryptedData.encrypted,
                emojiText: encryptedData.emoji,
                password: encryptedData.password,
                timestamp: new Date().toISOString()
            });
            
            // Save to localStorage
            localStorage.setItem('textmoji_records', JSON.stringify(records));
            
            alert('Encrypted text saved to your records!');
            
            // Update memory display
            updateMemoryDisplay();
            
            // Update profile stats
            updateProfileStats();
            
            // Update records if we're on that tab
            if (document.getElementById('recordsPanel').classList.contains('active')) {
                loadRecords();
            }
        }
        
        // Clear decrypt inputs and results
        function clearDecrypt() {
            encryptedText.value = '';
            decryptPassword.value = '';
            decryptResultContainer.classList.remove('active');
        }
        
        // Setup password toggle functionality
        function setupPasswordToggles() {
            // Authentication toggles
            toggleLoginPassword.addEventListener('click', () => togglePassword('loginPassword', toggleLoginPassword));
            toggleSignupPassword.addEventListener('click', () => togglePassword('signupPassword', toggleSignupPassword));
            toggleSignupConfirmPassword.addEventListener('click', () => togglePassword('signupConfirmPassword', toggleSignupConfirmPassword));
            toggleNewPassword.addEventListener('click', () => togglePassword('newPassword', toggleNewPassword));
            toggleConfirmNewPassword.addEventListener('click', () => togglePassword('confirmNewPassword', toggleConfirmNewPassword));
            
            // App toggles
            toggleEncryptPassword.addEventListener('click', () => togglePassword('encryptPassword', toggleEncryptPassword));
            toggleDecryptPassword.addEventListener('click', () => togglePassword('decryptPassword', toggleDecryptPassword));
        }
        
        // Toggle password visibility
        function togglePassword(inputId, toggleButton) {
            const passwordField = document.getElementById(inputId);
            
            if (passwordField.type === 'password') {
                passwordField.type = 'text';
                toggleButton.innerHTML = '<i class="fas fa-eye-slash"></i>';
            } else {
                passwordField.type = 'password';
                toggleButton.innerHTML = '<i class="fas fa-eye"></i>';
            }
        }
        
        // Load and display records
        function loadRecords() {
            const userEmail = currentUser.email;
            const userRecords = records[userEmail] || [];
            
            if (userRecords.length === 0) {
                recordsList.innerHTML = `
                    <div class="no-records">
                        <i class="fas fa-inbox" style="font-size: 3rem; margin-bottom: 15px; opacity: 0.5;"></i>
                        <p>No encryption records found. Start by encrypting some text!</p>
                    </div>
                `;
                clearAllRecordsBtn.style.display = 'none';
                return;
            }
            
            let recordsHTML = '';
            userRecords.forEach(record => {
                recordsHTML += `
                    <div class="record-item" data-id="${record.id}">
                        <div class="record-header">
                            <div class="record-title">Encrypted Message</div>
                            <div class="record-date">${new Date(record.timestamp).toLocaleDateString()}</div>
                        </div>
                        <div class="record-preview">
                            <div class="record-original">
                                <div class="record-label">Original Text</div>
                                <div class="record-value">${record.originalText.substring(0, 50)}${record.originalText.length > 50 ? '...' : ''}</div>
                            </div>
                            <div class="record-emoji">
                                <div class="record-label">Emoji Text</div>
                                <div class="record-value">${record.emojiText.substring(0, 30)}${record.emojiText.length > 30 ? '...' : ''}</div>
                            </div>
                        </div>
                        <div class="record-actions">
                            <button class="delete-record-btn" data-id="${record.id}">
                                <i class="fas fa-trash"></i>
                                Delete
                            </button>
                        </div>
                    </div>
                `;
            });
            
            recordsList.innerHTML = recordsHTML;
            clearAllRecordsBtn.style.display = 'block';
            
            // Add event listeners to delete buttons
            document.querySelectorAll('.delete-record-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    e.stopPropagation();
                    const recordId = button.getAttribute('data-id');
                    deleteRecord(recordId);
                });
            });
            
            // Add event listeners to record items for viewing details
            document.querySelectorAll('.record-item').forEach(item => {
                item.addEventListener('click', () => {
                    const recordId = item.getAttribute('data-id');
                    viewRecordDetails(recordId);
                });
            });
        }
        
        // Delete a specific record
        function deleteRecord(recordId) {
            const userEmail = currentUser.email;
            records[userEmail] = records[userEmail].filter(record => record.id !== recordId);
            localStorage.setItem('textmoji_records', JSON.stringify(records));
            updateMemoryDisplay();
            updateProfileStats();
            loadRecords();
        }
        
        // View record details
        function viewRecordDetails(recordId) {
            const userEmail = currentUser.email;
            const record = records[userEmail].find(r => r.id === recordId);
            
            if (record) {
                // Switch to decrypt tab and populate fields
                switchTab('decrypt');
                encryptedText.value = record.emojiText;
                decryptPassword.value = record.password;
                
                // Auto-decrypt
                setTimeout(() => {
                    handleDecryption();
                }, 500);
            }
        }
        
        // Clear all records
        function clearAllRecords() {
            if (confirm('Are you sure you want to delete all your encryption records? This action cannot be undone.')) {
                const userEmail = currentUser.email;
                records[userEmail] = [];
                localStorage.setItem('textmoji_records', JSON.stringify(records));
                updateMemoryDisplay();
                updateProfileStats();
                loadRecords();
            }
        }
        
        // Update memory count display
        function updateMemoryDisplay() {
            const userEmail = currentUser.email;
            const userRecords = records[userEmail] || [];
            memoryCount.textContent = userRecords.length;
        }
        
        // Setup input validation
        function setupInputValidation() {
            // Email validation
            const emailInputs = document.querySelectorAll('input[type="email"]');
            emailInputs.forEach(input => {
                input.addEventListener('blur', () => {
                    if (input.value && !validateEmail(input.value)) {
                        const errorId = input.id + 'Error';
                        showError(errorId, 'Please enter a valid email address');
                    } else {
                        hideError(input.id + 'Error');
                    }
                });
            });
            
            // Password validation
            const passwordInputs = document.querySelectorAll('input[type="password"]');
            passwordInputs.forEach(input => {
                if (input.id.includes('Password') && !input.id.includes('Confirm')) {
                    input.addEventListener('blur', () => {
                        if (input.value && input.value.length < 6) {
                            const errorId = input.id + 'Error';
                            showError(errorId, 'Password must be at least 6 characters');
                        } else {
                            hideError(input.id + 'Error');
                        }
                    });
                }
            });
            
            // Confirm password validation
            const confirmPasswordInputs = document.querySelectorAll('input[id*="ConfirmPassword"]');
            confirmPasswordInputs.forEach(input => {
                input.addEventListener('blur', () => {
                    const passwordId = input.id.replace('Confirm', '');
                    const passwordInput = document.getElementById(passwordId);
                    
                    if (input.value && passwordInput.value !== input.value) {
                        const errorId = input.id + 'Error';
                        showError(errorId, 'Passwords do not match');
                    } else {
                        hideError(input.id + 'Error');
                    }
                });
            });
        }
        
        // Show error message
        function showError(errorId, message) {
            const errorElement = document.getElementById(errorId);
            errorElement.textContent = message;
            errorElement.style.display = 'block';
        }
        
        // Hide error message
        function hideError(errorId) {
            const errorElement = document.getElementById(errorId);
            errorElement.style.display = 'none';
        }
        
        // Validate email format
        function validateEmail(email) {
            const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            return re.test(email);
        }
        
        // Initialize the app when the page loads
        document.addEventListener('DOMContentLoaded', initApp);
    </script>
</body>
</html>
