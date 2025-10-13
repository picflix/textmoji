<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TextMoji - Secure Emoji Encryption</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/4.1.1/crypto-js.min.js"></script>
    <style>
        :root {
            --primary: #8B5CF6;
            --primary-dark: #7C3AED;
            --secondary: #06D6A0;
            --accent: #EF476F;
            --dark: #0F0F1E;
            --darker: #070711;
            --light: #F8FAFC;
            --gray: #94A3B8;
            --glass-bg: rgba(255, 255, 255, 0.05);
            --glass-border: rgba(255, 255, 255, 0.1);
            --glass-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.36);
            --transition: all 0.3s ease;
        }

        .light-theme {
            --dark: #F8FAFC;
            --darker: #E2E8F0;
            --light: #0F0F1E;
            --gray: #475569;
            --glass-bg: rgba(0, 0, 0, 0.05);
            --glass-border: rgba(0, 0, 0, 0.1);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', system-ui, sans-serif;
        }

        body {
            background: linear-gradient(135deg, var(--darker), var(--dark));
            color: var(--light);
            min-height: 100vh;
            transition: var(--transition);
        }

        .container {
            display: flex;
            min-height: 100vh;
            width: 100%;
        }

        /* =========================
           AUTH PAGES - FROM PERSONAL FINANCE TOOL
           ========================= */
        .auth-page {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: 
                linear-gradient(rgba(15, 15, 30, 0.9), rgba(15, 15, 30, 0.9)),
                url('https://images.unsplash.com/photo-1635070041078-e363dbe005cb?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80') no-repeat center center;
            background-size: cover;
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 9999;
            overflow: hidden;
        }
        
        .auth-container {
            position: relative;
            width: 100%;
            max-width: 450px;
            padding: 50px 40px;
            background: var(--glass-bg);
            backdrop-filter: blur(15px);
            border-radius: 24px;
            box-shadow: var(--glass-shadow);
            border: 1px solid var(--glass-border);
            border-right: 1px solid rgba(255, 255, 255, 0.1);
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            overflow: hidden;
            animation: fadeIn 0.8s cubic-bezier(0.39, 0.575, 0.565, 1);
        }
        
        .auth-container::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(
                90deg,
                transparent,
                rgba(255, 255, 255, 0.2),
                transparent
            );
            transition: 0.5s;
        }
        
        .auth-container:hover::before {
            left: 100%;
        }
        
        .auth-form {
            position: relative;
            width: 100%;
            display: flex;
            flex-direction: column;
            gap: 25px;
            align-items: center;
            z-index: 10;
        }
        
        .auth-form h2 {
            color: #fff;
            font-size: 2.5rem;
            font-weight: 700;
            letter-spacing: 1px;
            margin-bottom: 20px;
            text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
            position: relative;
            background: linear-gradient(to right, #fff, #c1c8ff);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-align: center;
            width: 100%;
            padding-bottom: 0;
            left: 0;
            transform: none;
            display: block;
        }
        
        .inputBx {
            position: relative;
            width: 100%;
            max-width: 350px;
        }
        
        .inputBx input {
            width: 100%;
            padding: 15px 25px;
            background: rgba(255, 255, 255, 0.2);
            border: none;
            border-radius: 12px;
            font-size: 1.1rem;
            color: #fff;
            outline: none;
            transition: all 0.5s ease;
            box-shadow: var(--glass-shadow);
            backdrop-filter: blur(5px);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .inputBx input:focus {
            border-color: #fff;
            box-shadow: 0 0 15px rgba(255, 255, 255, 0.3);
            transform: translateY(-2px);
        }
        
        .inputBx input[type="submit"] {
            background: linear-gradient(45deg, var(--primary), var(--primary-dark));
            border: none;
            cursor: pointer;
            font-weight: 600;
            letter-spacing: 1px;
            transition: all 0.5s ease;
            box-shadow: 0 5px 20px rgba(139, 92, 246, 0.4);
            margin-top: 10px;
            width: 100%;
            max-width: 350px;
        }
        
        .inputBx input[type="submit"]:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(139, 92, 246, 0.5);
            background: linear-gradient(45deg, var(--primary-dark), var(--primary));
        }
        
        .inputBx input::placeholder {
            color: rgba(255, 255, 255, 0.8);
        }
        
        .links {
            width: 100%;
            max-width: 350px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 10px;
        }
        
        .links a {
            color: #fff;
            text-decoration: none;
            font-size: 0.9rem;
            transition: all 0.3s ease;
            opacity: 0.8;
            position: relative;
            padding: 5px 0;
        }
        
        .links a:hover {
            opacity: 1;
            text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
        }
        
        .links a::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 0;
            height: 2px;
            background: #fff;
            transition: width 0.3s ease;
        }
        
        .links a:hover::after {
            width: 100%;
        }
        
        /* Error messages */
        .error-message {
            color: #ff4444;
            font-size: 0.9rem;
            margin-top: 8px;
            display: none;
            padding-left: 15px;
            animation: fadeIn 0.3s ease-out;
            text-shadow: 0 1px 2px rgba(0, 0, 0, 0.3);
            text-align: left;
            width: 100%;
        }

        /* Auth Error Alert */
        .auth-alert {
            padding: 15px 20px;
            border-radius: 12px;
            margin-bottom: 20px;
            display: none;
            transition: var(--transition);
            width: 100%;
            max-width: 350px;
            text-align: center;
            animation: shake 0.5s ease-in-out;
        }
        
        .auth-alert.error {
            background: rgba(239, 71, 111, 0.1);
            border-left: 4px solid var(--accent);
            color: var(--accent);
        }
        
        .auth-alert.success {
            background: rgba(6, 214, 160, 0.1);
            border-left: 4px solid var(--secondary);
            color: var(--secondary);
        }
        
        /* Password reset page */
        .reset-form {
            display: flex;
            flex-direction: column;
            gap: 20px;
            width: 100%;
            align-items: center;
        }
        
        .reset-steps {
            display: flex;
            flex-direction: column;
            gap: 20px;
            width: 100%;
            align-items: center;
        }
        
        .reset-step {
            display: none;
            animation: fadeIn 0.5s ease-out;
            width: 100%;
        }
        
        .reset-step.active {
            display: flex;
            flex-direction: column;
            gap: 20px;
            width: 100%;
            align-items: center;
        }
        
        /* Abstract floating shapes */
        .floating-shapes {
            position: absolute;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            z-index: 1;
            overflow: hidden;
        }
        
        .shape {
            position: absolute;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(5px);
            animation: float 15s infinite linear;
            opacity: 0.6;
        }
        
        .shape:nth-child(1) {
            width: 100px;
            height: 100px;
            top: 20%;
            left: 10%;
            animation-duration: 20s;
            animation-delay: 0s;
            background: linear-gradient(45deg, rgba(139, 92, 246, 0.1), rgba(6, 214, 160, 0.1));
        }
        
        .shape:nth-child(2) {
            width: 150px;
            height: 150px;
            top: 60%;
            left: 70%;
            animation-duration: 25s;
            animation-delay: 2s;
            background: linear-gradient(45deg, rgba(6, 214, 160, 0.1), rgba(239, 71, 111, 0.1));
        }
        
        .shape:nth-child(3) {
            width: 80px;
            height: 80px;
            top: 80%;
            left: 20%;
            animation-duration: 15s;
            animation-delay: 1s;
            background: linear-gradient(45deg, rgba(239, 71, 111, 0.1), rgba(139, 92, 246, 0.1));
        }
        
        .shape:nth-child(4) {
            width: 120px;
            height: 120px;
            top: 30%;
            left: 80%;
            animation-duration: 30s;
            animation-delay: 3s;
            background: linear-gradient(45deg, rgba(6, 214, 160, 0.1), rgba(139, 92, 246, 0.1));
        }
        
        /* Hidden class */
        .hidden {
            display: none;
        }

        /* Dashboard */
        .dashboard {
            display: none;
            width: 100%;
        }

        .sidebar {
            width: 280px;
            background: var(--glass-bg);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border-right: 1px solid var(--glass-border);
            height: 100vh;
            position: fixed;
            left: 0;
            top: 0;
            padding: 25px 20px;
            overflow-y: auto;
            z-index: 100;
        }

        .sidebar-header {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 40px;
            padding-bottom: 20px;
            border-bottom: 1px solid var(--glass-border);
        }

        .user-avatar {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 1.3rem;
        }

        .user-info h3 {
            font-size: 1.2rem;
            margin-bottom: 5px;
        }

        .user-info p {
            color: var(--gray);
            font-size: 0.9rem;
        }

        .nav-item {
            display: flex;
            align-items: center;
            gap: 15px;
            padding: 15px;
            margin-bottom: 8px;
            border-radius: 12px;
            cursor: pointer;
            transition: var(--transition);
            color: var(--light);
            text-decoration: none;
        }

        .nav-item:hover {
            background: rgba(255, 255, 255, 0.07);
        }

        .nav-item.active {
            background: linear-gradient(135deg, var(--primary), var(--primary-dark));
            color: white;
        }

        .nav-item i {
            font-size: 1.2rem;
            width: 24px;
            text-align: center;
        }

        .main-content {
            margin-left: 280px;
            padding: 30px;
            width: calc(100% - 280px);
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
        }

        .page-title {
            font-size: 2rem;
            font-weight: 700;
        }

        .header-actions {
            display: flex;
            gap: 15px;
        }

        .theme-toggle, .menu-toggle {
            background: var(--glass-bg);
            backdrop-filter: blur(10px);
            border: 1px solid var(--glass-border);
            border-radius: 12px;
            width: 50px;
            height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: var(--transition);
        }

        .theme-toggle:hover, .menu-toggle:hover {
            background: rgba(255, 255, 255, 0.1);
        }

        .card {
            background: var(--glass-bg);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid var(--glass-border);
            border-radius: 20px;
            box-shadow: var(--glass-shadow);
            padding: 30px;
            margin-bottom: 30px;
            transition: var(--transition);
        }

        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
        }

        .card-title {
            font-size: 1.5rem;
            font-weight: 700;
        }

        .tabs {
            display: flex;
            background: rgba(255, 255, 255, 0.07);
            border-radius: 12px;
            padding: 5px;
            margin-bottom: 25px;
        }

        .tab {
            flex: 1;
            text-align: center;
            padding: 12px;
            border-radius: 8px;
            cursor: pointer;
            transition: var(--transition);
            font-weight: 600;
        }

        .tab.active {
            background: linear-gradient(135deg, var(--primary), var(--primary-dark));
            color: white;
        }

        .result-container {
            margin-top: 25px;
            display: none;
        }

        .result-box {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 12px;
            padding: 20px;
            margin-top: 15px;
            border: 1px solid var(--glass-border);
            word-break: break-all;
            min-height: 80px;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .copy-btn {
            background: var(--glass-bg);
            border: 1px solid var(--glass-border);
            border-radius: 8px;
            color: var(--light);
            padding: 8px 15px;
            cursor: pointer;
            transition: var(--transition);
        }

        .copy-btn:hover {
            background: rgba(255, 255, 255, 0.1);
        }

        .emoji-display {
            font-size: 1.5rem;
            line-height: 2;
        }

        .alert {
            padding: 15px 20px;
            border-radius: 12px;
            margin-bottom: 20px;
            display: none;
            transition: var(--transition);
        }

        .alert-success {
            background: rgba(6, 214, 160, 0.1);
            border-left: 4px solid var(--secondary);
            color: var(--secondary);
        }

        .alert-error {
            background: rgba(239, 71, 111, 0.1);
            border-left: 4px solid var(--accent);
            color: var(--accent);
        }

        .history-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }

        .history-table th, .history-table td {
            padding: 15px;
            text-align: left;
            border-bottom: 1px solid var(--glass-border);
        }

        .history-table th {
            color: var(--gray);
            font-weight: 600;
        }

        .history-table tr:hover {
            background: rgba(255, 255, 255, 0.03);
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            backdrop-filter: blur(5px);
            z-index: 1000;
            align-items: center;
            justify-content: center;
        }

        .modal-content {
            background: var(--glass-bg);
            backdrop-filter: blur(16px);
            border: 1px solid var(--glass-border);
            border-radius: 20px;
            width: 90%;
            max-width: 500px;
            padding: 30px;
            box-shadow: var(--glass-shadow);
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .modal-title {
            font-size: 1.5rem;
            font-weight: 700;
        }

        .close-modal {
            background: none;
            border: none;
            color: var(--gray);
            font-size: 1.5rem;
            cursor: pointer;
        }

        .menu-toggle {
            display: none;
        }

        .password-feedback {
            margin-top: 8px;
            font-size: 0.85rem;
            display: none;
        }

        .password-feedback.error {
            color: var(--accent);
        }

        .password-feedback.success {
            color: var(--secondary);
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: var(--light);
        }

        .form-control {
            width: 100%;
            padding: 12px 16px;
            background: rgba(255, 255, 255, 0.07);
            border: 1px solid var(--glass-border);
            border-radius: 12px;
            color: var(--light);
            font-size: 1rem;
            transition: var(--transition);
        }

        .form-control:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(139, 92, 246, 0.2);
        }

        .btn {
            display: inline-flex;
            align-items: center;
            gap: 8px;
            padding: 12px 24px;
            border: none;
            border-radius: 12px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: var(--transition);
            text-decoration: none;
        }

        .btn-primary {
            background: linear-gradient(135deg, var(--primary), var(--primary-dark));
            color: white;
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(139, 92, 246, 0.4);
        }

        .password-input {
            position: relative;
        }

        .toggle-password {
            position: absolute;
            right: 12px;
            top: 50%;
            transform: translateY(-50%);
            background: none;
            border: none;
            color: var(--gray);
            cursor: pointer;
        }

        @keyframes float {
            0% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
            100% { transform: translateY(0px); }
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            10%, 30%, 50%, 70%, 90% { transform: translateX(-5px); }
            20%, 40%, 60%, 80% { transform: translateX(5px); }
        }

        @media (max-width: 768px) {
            .sidebar {
                transform: translateX(-100%);
                transition: transform 0.3s ease;
            }
            
            .sidebar.active {
                transform: translateX(0);
            }
            
            .main-content {
                margin-left: 0;
                width: 100%;
                padding: 20px 15px;
            }
            
            .menu-toggle {
                display: flex;
            }

            .auth-container {
                width: 90%;
                padding: 40px 20px;
            }
            
            .auth-form h2 {
                font-size: 2rem;
            }
            
            .inputBx input {
                padding: 12px 20px;
            }
            
            .links {
                flex-direction: column;
                gap: 10px;
                align-items: center;
            }
            
            .card {
                padding: 20px;
            }
        }
    </style>
</head>
<body>
    <!-- LOGIN PAGE -->
    <div id="loginPage" class="auth-page">
        <div class="auth-container">
            <!-- Floating abstract shapes -->
            <div class="floating-shapes">
                <div class="shape"></div>
                <div class="shape"></div>
                <div class="shape"></div>
                <div class="shape"></div>
            </div>
            
            <div class="auth-form">
                <h2>Welcome</h2>
                
                <!-- Auth Alert -->
                <div class="auth-alert error" id="loginAlert" style="display: none;">
                    <i class="fas fa-exclamation-circle"></i> <span id="loginAlertMessage">Invalid username or password</span>
                </div>
                
                <div class="inputBx">
                    <input type="text" placeholder="Username" id="loginUsername" />
                    <div id="loginUsernameError" class="error-message">Username is required</div>
                </div>
                <div class="inputBx">
                    <input type="password" placeholder="Password" id="loginPassword" />
                    <div id="loginPasswordError" class="error-message">Password is required</div>
                </div>
                <div class="inputBx">
                    <input type="submit" value="Sign in" onclick="login()" />
                </div>
                <div class="links">
                    <a href="#" onclick="showRegisterPage()">Create Account</a>
                    <a href="#" onclick="showForgotPasswordPage()">Forgot Password?</a>
                </div>
            </div>
        </div>
    </div>

    <!-- REGISTER PAGE -->
    <div id="registerPage" class="auth-page hidden">
        <div class="auth-container">
            <!-- Floating abstract shapes -->
            <div class="floating-shapes">
                <div class="shape"></div>
                <div class="shape"></div>
                <div class="shape"></div>
                <div class="shape"></div>
            </div>
            
            <div class="auth-form">
                <h2>Create Account</h2>
                
                <!-- Auth Alert -->
                <div class="auth-alert success" id="registerAlert" style="display: none;">
                    <i class="fas fa-check-circle"></i> <span id="registerAlertMessage">Registration successful!</span>
                </div>
                
                <div class="inputBx">
                    <input type="text" placeholder="Username" id="regUsername" />
                    <div id="regUsernameError" class="error-message">Username is required</div>
                </div>
                <div class="inputBx">
                    <input type="email" placeholder="Email" id="regEmail" />
                    <div id="regEmailError" class="error-message">Valid email is required</div>
                </div>
                <div class="inputBx">
                    <input type="password" placeholder="Password" id="regPassword" />
                    <div id="regPasswordError" class="error-message">Password must be at least 8 characters</div>
                </div>
                <div class="inputBx">
                    <input type="password" placeholder="Confirm Password" id="regConfirmPassword" />
                    <div id="regConfirmPasswordError" class="error-message">Passwords must match</div>
                </div>
                <div class="inputBx">
                    <input type="submit" value="Register" onclick="register()" />
                </div>
                <div class="links">
                    <a href="#" onclick="showLoginPage()">Already have an account? Login</a>
                </div>
            </div>
        </div>
    </div>

    <!-- FORGOT PASSWORD PAGE -->
    <div id="forgotPasswordPage" class="auth-page hidden">
        <div class="auth-container">
            <!-- Floating abstract shapes -->
            <div class="floating-shapes">
                <div class="shape"></div>
                <div class="shape"></div>
                <div class="shape"></div>
                <div class="shape"></div>
            </div>
            
            <div class="auth-form">
                <div class="reset-form">
                    <h2>Reset Password</h2>
                    
                    <!-- Auth Alert -->
                    <div class="auth-alert" id="resetAlert" style="display: none;">
                        <i class="fas fa-info-circle"></i> <span id="resetAlertMessage">Reset link sent to your email</span>
                    </div>
                    
                    <!-- Step 1: Enter Email -->
                    <div id="resetStep1" class="reset-step active">
                        <h3 style="color: white; text-align: center; font-weight: 300; margin-bottom: 15px;">Enter your registered email address</h3>
                        <div class="inputBx">
                            <input type="email" placeholder="Email" id="resetEmail" />
                            <div id="resetEmailError" class="error-message">Valid email is required</div>
                        </div>
                        <div class="inputBx">
                            <input type="submit" value="Send Reset Link" onclick="verifyResetEmail()" />
                        </div>
                        <div class="links">
                            <a href="#" onclick="showLoginPage()">Back to Login</a>
                        </div>
                    </div>
                    
                    <!-- Step 2: Enter New Password -->
                    <div id="resetStep2" class="reset-step">
                        <h3 style="color: white; text-align: center; font-weight: 300; margin-bottom: 15px;">Create New Password</h3>
                        <div class="inputBx">
                            <input type="password" placeholder="New Password" id="newPassword" />
                            <div id="newPasswordError" class="error-message">Password must be at least 8 characters</div>
                        </div>
                        <div class="inputBx">
                            <input type="password" placeholder="Confirm New Password" id="confirmNewPassword" />
                            <div id="confirmNewPasswordError" class="error-message">Passwords must match</div>
                        </div>
                        <div class="inputBx">
                            <input type="submit" value="Update Password" onclick="resetPassword()" />
                        </div>
                        <div class="links">
                            <a href="#" onclick="showLoginPage()">Back to Login</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Dashboard -->
    <div class="dashboard" id="dashboard">
        <div class="sidebar">
            <div class="sidebar-header">
                <div class="user-avatar" id="user-avatar">JD</div>
                <div class="user-info">
                    <h3 id="user-name">John Doe</h3>
                    <p id="user-email">john.doe@example.com</p>
                </div>
            </div>
            
            <a href="#" class="nav-item active" data-tab="encryption">
                <i class="fas fa-lock"></i>
                <span>Encrypt Text</span>
            </a>
            <a href="#" class="nav-item" data-tab="decryption">
                <i class="fas fa-unlock"></i>
                <span>Decrypt Emojis</span>
            </a>
            <a href="#" class="nav-item" id="history-tab">
                <i class="fas fa-history"></i>
                <span>History</span>
            </a>
            <a href="#" class="nav-item" id="settings-tab">
                <i class="fas fa-cog"></i>
                <span>Settings</span>
            </a>
            <a href="#" class="nav-item" id="logout-btn">
                <i class="fas fa-sign-out-alt"></i>
                <span>Log Out</span>
            </a>
        </div>
        
        <div class="main-content">
            <div class="header">
                <h1 class="page-title" id="page-title">Encrypt Text</h1>
                <div class="header-actions">
                    <div class="theme-toggle" id="theme-toggle">
                        <i class="fas fa-moon"></i>
                    </div>
                    <div class="menu-toggle" id="menu-toggle">
                        <i class="fas fa-bars"></i>
                    </div>
                </div>
            </div>
            
            <!-- Encryption Card -->
            <div class="card" id="encryption-card">
                <div class="card-header">
                    <h2 class="card-title">Text Encryption</h2>
                </div>
                
                <div class="form-group">
                    <label class="form-label">Enter your message</label>
                    <textarea class="form-control" id="textmsg" placeholder="Type your secret message here..." rows="4"></textarea>
                </div>
                
                <div class="form-group">
                    <label class="form-label">Set encryption password</label>
                    <div class="password-input">
                        <input type="password" class="form-control" id="password" placeholder="Enter a strong password">
                        <button type="button" class="toggle-password" id="toggle-password">
                            <i class="far fa-eye"></i>
                        </button>
                    </div>
                    <div class="password-feedback" id="encrypt-password-feedback"></div>
                </div>
                
                <button class="btn btn-primary" id="encrypt-btn">
                    <i class="fas fa-lock"></i> Encrypt Message
                </button>
                
                <div class="result-container" id="encrypt-result">
                    <div class="form-label">Encrypted Message</div>
                    <div class="result-box">
                        <div class="emoji-display" id="encrypted-output"></div>
                        <button class="copy-btn" id="copy-encrypted"><i class="far fa-copy"></i> Copy</button>
                    </div>
                </div>
            </div>
            
            <!-- Decryption Card -->
            <div class="card" id="decryption-card" style="display: none;">
                <div class="card-header">
                    <h2 class="card-title">Emoji Decryption</h2>
                </div>
                
                <div class="form-group">
                    <label class="form-label">Paste encrypted emojis</label>
                    <textarea class="form-control" id="emojimsg" placeholder="Paste your emoji message here..." rows="4"></textarea>
                </div>
                
                <div class="form-group">
                    <label class="form-label">Enter decryption password</label>
                    <div class="password-input">
                        <input type="password" class="form-control" id="finalpassword" placeholder="Enter the password used for encryption">
                        <button type="button" class="toggle-password" id="toggle-finalpassword">
                            <i class="far fa-eye"></i>
                        </button>
                    </div>
                    <div class="password-feedback" id="decrypt-password-feedback"></div>
                </div>
                
                <button class="btn btn-primary" id="decrypt-btn">
                    <i class="fas fa-unlock"></i> Decrypt Message
                </button>
                
                <div class="result-container" id="decrypt-result">
                    <div class="form-label">Decrypted Message</div>
                    <div class="result-box">
                        <div id="decrypted-output"></div>
                        <button class="copy-btn" id="copy-decrypted"><i class="far fa-copy"></i> Copy</button>
                    </div>
                </div>
            </div>
            
            <!-- History Card -->
            <div class="card" id="history-card" style="display: none;">
                <div class="card-header">
                    <h2 class="card-title">Encryption History</h2>
                </div>
                
                <table class="history-table">
                    <thead>
                        <tr>
                            <th>Date</th>
                            <th>Operation</th>
                            <th>Message Preview</th>
                            <th>Status</th>
                        </tr>
                    </thead>
                    <tbody id="history-body">
                        <!-- History will be populated by JavaScript -->
                    </tbody>
                </table>
            </div>
            
            <!-- Alerts -->
            <div class="alert alert-success" id="success-alert">
                <i class="fas fa-check-circle"></i> <span id="success-message">Operation completed successfully!</span>
            </div>
            
            <div class="alert alert-error" id="error-alert">
                <i class="fas fa-exclamation-circle"></i> <span id="error-message">An error occurred</span>
            </div>
        </div>
    </div>
    
    <!-- Settings Modal -->
    <div class="modal" id="settings-modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2 class="modal-title">Settings</h2>
                <button class="close-modal" id="close-settings">&times;</button>
            </div>
            
            <div class="form-group">
                <label class="form-label">Theme</label>
                <select class="form-control" id="theme-select">
                    <option value="dark">Dark</option>
                    <option value="light">Light</option>
                    <option value="system">System</option>
                </select>
            </div>
            
            <div class="form-group">
                <label class="form-label">Auto-logout after</label>
                <select class="form-control" id="logout-time">
                    <option value="15">15 minutes</option>
                    <option value="30">30 minutes</option>
                    <option value="60">1 hour</option>
                    <option value="0">Never</option>
                </select>
            </div>
            
            <div class="form-group">
                <label class="form-label">Clear History</label>
                <button class="btn" id="clear-history" style="background: var(--accent);">Clear All History</button>
            </div>
            
            <div class="form-group">
                <label class="form-label">Delete Account</label>
                <button class="btn" id="delete-account" style="background: var(--accent);">Delete My Account</button>
            </div>
            
            <button class="btn btn-primary" id="save-settings">Save Settings</button>
        </div>
    </div>

    <!-- Verification Modal for History -->
    <div class="modal" id="verify-modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2 class="modal-title">Verify Identity</h2>
                <button class="close-modal" id="close-verify">&times;</button>
            </div>
            
            <div class="form-group">
                <label class="form-label">Username</label>
                <input type="text" class="form-control" id="verify-username" placeholder="Enter your username">
            </div>
            
            <div class="form-group">
                <label class="form-label">Password</label>
                <input type="password" class="form-control" id="verify-password" placeholder="Enter your password">
            </div>
            
            <div class="error-message" id="verify-error" style="display: none;"></div>
            
            <button class="btn btn-primary" id="verify-submit">Verify</button>
        </div>
    </div>

    <script>
        // =======================================================
        // AUTHENTICATION SYSTEM - FROM PERSONAL FINANCE TOOL
        // =======================================================

        // -------------------------------------------------------
        // UTILITY FUNCTIONS
        // -------------------------------------------------------
        function showError(elementId, message) {
            const element = document.getElementById(elementId);
            element.textContent = message;
            element.style.display = 'block';
            return false;
        }
        
        function hideError(elementId) {
            const element = document.getElementById(elementId);
            element.style.display = 'none';
            return true;
        }
        
        function showAuthAlert(type, message, alertId) {
            const alert = document.getElementById(alertId);
            alert.className = `auth-alert ${type}`;
            document.getElementById(`${alertId}Message`).textContent = message;
            alert.style.display = 'block';
            
            // Auto-hide after 5 seconds
            setTimeout(() => {
                alert.style.display = 'none';
            }, 5000);
        }
        
        function validateEmail(email) {
            const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            return re.test(email);
        }
        
        function getCurrentTimestamp() {
            return new Date().toISOString();
        }
        
        // Simple hash function for password obfuscation
        function simpleHash(str) {
            let hash = 0;
            for (let i = 0; i < str.length; i++) {
                const char = str.charCodeAt(i);
                hash = (hash << 5) - hash + char;
                hash = hash & hash;
            }
            return hash.toString();
        }
        
        // -------------------------------------------------------
        // PAGE NAVIGATION FUNCTIONS - FIXED
        // -------------------------------------------------------
        function showRegisterPage() {
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('registerPage').classList.remove('hidden');
            document.getElementById('forgotPasswordPage').classList.add('hidden');
            // Don't hide dashboard here
        }
        
        function showLoginPage() {
            document.getElementById('registerPage').classList.add('hidden');
            document.getElementById('loginPage').classList.remove('hidden');
            document.getElementById('forgotPasswordPage').classList.add('hidden');
            document.getElementById('dashboard').classList.add('hidden');
        }
        
        function showForgotPasswordPage() {
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('registerPage').classList.add('hidden');
            document.getElementById('forgotPasswordPage').classList.remove('hidden');
            document.getElementById('dashboard').classList.add('hidden');
        }
        
        function showAppPage() {
            // Hide all auth pages
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('registerPage').classList.add('hidden');
            document.getElementById('forgotPasswordPage').classList.add('hidden');
            
            // Show main app page
            document.getElementById('dashboard').classList.remove('hidden');
            
            // Initialize dashboard components
            initializeDashboard();
            
            // Update username
            const username = localStorage.getItem('currentUser');
            if (username) {
                document.getElementById('user-name').textContent = username;
                // Get user email from stored data
                const users = JSON.parse(localStorage.getItem('textmoji_users')) || {};
                const user = users[username];
                if (user && user.email) {
                    document.getElementById('user-email').textContent = user.email;
                }
                document.getElementById('user-avatar').textContent = username.substring(0, 2).toUpperCase();
            }
        }
        
        // -------------------------------------------------------
        // DASHBOARD INITIALIZATION - NEW FUNCTION
        // -------------------------------------------------------
        function initializeDashboard() {
            // Set up navigation tabs
            const navItems = document.querySelectorAll('.nav-item[data-tab]');
            navItems.forEach(item => {
                item.addEventListener('click', function(e) {
                    e.preventDefault();
                    
                    // Remove active class from all items
                    navItems.forEach(i => i.classList.remove('active'));
                    
                    // Add active class to clicked item
                    this.classList.add('active');
                    
                    const tab = this.getAttribute('data-tab');
                    if (tab) {
                        // Update page title
                        document.getElementById('page-title').textContent = 
                            tab === 'encryption' ? 'Encrypt Text' : 
                            tab === 'decryption' ? 'Decrypt Emojis' : 'History';
                        
                        // Show/hide appropriate cards
                        document.getElementById('encryption-card').style.display = 
                            tab === 'encryption' ? 'block' : 'none';
                        document.getElementById('decryption-card').style.display = 
                            tab === 'decryption' ? 'block' : 'none';
                        document.getElementById('history-card').style.display = 
                            tab === 'history' ? 'block' : 'none';
                        
                        if (tab === 'history') {
                            loadHistory();
                        }
                    }
                });
            });
            
            // Ensure encryption tab is active by default
            const encryptionTab = document.querySelector('[data-tab="encryption"]');
            if (encryptionTab) {
                encryptionTab.classList.add('active');
            }
            document.getElementById('encryption-card').style.display = 'block';
            document.getElementById('decryption-card').style.display = 'none';
            document.getElementById('history-card').style.display = 'none';
        }
        
        // -------------------------------------------------------
        // PASSWORD RESET FUNCTIONS
        // -------------------------------------------------------
        let resetEmail = ''; // Store the email being reset globally
        
        function verifyResetEmail() {
            const email = document.getElementById('resetEmail').value.trim();
            
            if (!email) {
                showError('resetEmailError', 'Email is required');
                return;
            }
            
            if (!validateEmail(email)) {
                showError('resetEmailError', 'Please enter a valid email');
                return;
            }
            
            // Check if email exists in users
            const users = JSON.parse(localStorage.getItem('textmoji_users')) || {};
            const user = Object.values(users).find(u => u.email === email);
            
            if (!user) {
                showError('resetEmailError', 'No account found with this email');
                return;
            }
            
            // Store the email for the next step
            resetEmail = email;
            
            // Move to step 2
            document.getElementById('resetStep1').classList.remove('active');
            document.getElementById('resetStep2').classList.add('active');
            
            // Show success message
            showAuthAlert('success', 'Reset link sent to your email', 'resetAlert');
        }
        
        function resetPassword() {
            const newPassword = document.getElementById('newPassword').value;
            const confirmPassword = document.getElementById('confirmNewPassword').value;
            
            // Reset error messages
            hideError('newPasswordError');
            hideError('confirmNewPasswordError');
            
            // Validate inputs
            let isValid = true;
            
            if (!newPassword) {
                showError('newPasswordError', 'Password is required');
                isValid = false;
            } else if (newPassword.length < 8) {
                showError('newPasswordError', 'Password must be at least 8 characters');
                isValid = false;
            }
            
            if (newPassword !== confirmPassword) {
                showError('confirmNewPasswordError', 'Passwords do not match');
                isValid = false;
            }
            
            if (!isValid) return;
            
            // Update password in users
            const users = JSON.parse(localStorage.getItem('textmoji_users'));
            const username = Object.keys(users).find(u => users[u].email === resetEmail);
            
            if (username) {
                users[username].password = simpleHash(newPassword);
                localStorage.setItem('textmoji_users', JSON.stringify(users));
                
                showAuthAlert('success', 'Password reset successfully! Please login with your new password.', 'resetAlert');
                setTimeout(() => {
                    showLoginPage();
                }, 2000);
            } else {
                showAuthAlert('error', 'Error resetting password. Please try again.', 'resetAlert');
            }
        }
        
        // -------------------------------------------------------
        // USER MANAGEMENT SYSTEM
        // -------------------------------------------------------
        function register() {
            // Reset error messages
            hideError('regUsernameError');
            hideError('regEmailError');
            hideError('regPasswordError');
            hideError('regConfirmPasswordError');
            
            const username = document.getElementById('regUsername').value.trim();
            const email = document.getElementById('regEmail').value.trim();
            const password = document.getElementById('regPassword').value;
            const confirmPassword = document.getElementById('regConfirmPassword').value;
            
            // Validate inputs
            let isValid = true;
            
            if (!username) {
                showError('regUsernameError', 'Username is required');
                isValid = false;
            }
            
            if (!email) {
                showError('regEmailError', 'Email is required');
                isValid = false;
            } else if (!validateEmail(email)) {
                showError('regEmailError', 'Please enter a valid email');
                isValid = false;
            }
            
            if (!password) {
                showError('regPasswordError', 'Password is required');
                isValid = false;
            } else if (password.length < 8) {
                showError('regPasswordError', 'Password must be at least 8 characters');
                isValid = false;
            }
            
            if (password !== confirmPassword) {
                showError('regConfirmPasswordError', 'Passwords do not match');
                isValid = false;
            }
            
            if (!isValid) return;
            
            // Get existing users or create new list
            const users = JSON.parse(localStorage.getItem('textmoji_users')) || {};
            
            if (users[username]) {
                showError('regUsernameError', 'Username already exists');
                return;
            }
            
            // Check if email is already registered
            const emailExists = Object.values(users).some(user => user.email === email);
            if (emailExists) {
                showError('regEmailError', 'Email already registered');
                return;
            }
            
            // Store new user with hashed password
            users[username] = { 
                email: email,
                password: simpleHash(password),
                data: {
                    encryptionHistory: []
                },
                settings: {
                    theme: "dark",
                    autoLogout: "30"
                },
                drafts: [],
                createdAt: getCurrentTimestamp()
            };
            
            localStorage.setItem('textmoji_users', JSON.stringify(users));
            
            showAuthAlert('success', 'Registration successful! Please login.', 'registerAlert');
            
            // Redirect to login page after 2 seconds
            setTimeout(() => {
                showLoginPage();
            }, 2000);
        }
        
        function login() {
            // Reset error messages
            hideError('loginUsernameError');
            hideError('loginPasswordError');
            
            const username = document.getElementById('loginUsername').value.trim();
            const password = document.getElementById('loginPassword').value;
            
            // Validate inputs
            let isValid = true;
            
            if (!username) {
                showError('loginUsernameError', 'Username is required');
                isValid = false;
            }
            
            if (!password) {
                showError('loginPasswordError', 'Password is required');
                isValid = false;
            }
            
            if (!isValid) return;
            
            const users = JSON.parse(localStorage.getItem('textmoji_users')) || {};
            const user = users[username];
            
            // Check if user exists and password matches
            if (user && user.password === simpleHash(password)) {
                // Login successful
                localStorage.setItem('currentUser', username);
                
                // Show success message
                showAuthAlert('success', 'Login successful! Redirecting...', 'loginAlert');
                
                // Redirect to app page after 1 second
                setTimeout(() => {
                    showAppPage();
                }, 1000);
            } else {
                // Show error message
                showAuthAlert('error', 'Invalid username or password', 'loginAlert');
                
                // Clear password field
                document.getElementById('loginPassword').value = '';
            }
        }
        
        function logout() {
            // Save user data before logout
            saveUserData();
            localStorage.removeItem('currentUser');
            location.reload();
        }
        
        function saveUserData() {
            const username = localStorage.getItem('currentUser');
            if (!username) return;
            
            const users = JSON.parse(localStorage.getItem('textmoji_users'));
            if (users && users[username]) {
                // Save encryption history to user data
                const encryptionHistory = JSON.parse(localStorage.getItem('encryptionHistory')) || [];
                users[username].data = {
                    encryptionHistory: encryptionHistory
                };
                
                localStorage.setItem('textmoji_users', JSON.stringify(users));
            }
        }
        
        // =======================================================
        // AES-256-GCM ENCRYPTION SYSTEM
        // =======================================================
        
        class AES256GCM {
            constructor() {
                this.AES_KEY_LENGTH = 256;
                this.GCM_IV_LENGTH = 12;
                this.GCM_TAG_LENGTH = 16;
            }

            async encrypt(plaintext, password) {
                try {
                    // Derive key from password using PBKDF2
                    const salt = CryptoJS.lib.WordArray.random(128/8);
                    const key = CryptoJS.PBKDF2(password, salt, {
                        keySize: 256/32,
                        iterations: 1000
                    });
                    
                    // Generate a random IV
                    const iv = CryptoJS.lib.WordArray.random(this.GCM_IV_LENGTH);
                    
                    // Encrypt using AES-GCM
                    const encrypted = CryptoJS.AES.encrypt(plaintext, key, {
                        iv: iv,
                        mode: CryptoJS.mode.GCM,
                        padding: CryptoJS.pad.Pkcs7
                    });
                    
                    // Combine salt + iv + ciphertext
                    const combined = salt.toString() + iv.toString() + encrypted.ciphertext.toString();
                    
                    return combined;
                } catch (error) {
                    console.error('Encryption error:', error);
                    throw new Error('Encryption failed');
                }
            }

            async decrypt(ciphertext, password) {
                try {
                    // Extract salt, iv and ciphertext
                    const salt = CryptoJS.enc.Hex.parse(ciphertext.substr(0, 32));
                    const iv = CryptoJS.enc.Hex.parse(ciphertext.substr(32, 24));
                    const ct = CryptoJS.enc.Hex.parse(ciphertext.substr(56));
                    
                    // Derive key from password using PBKDF2
                    const key = CryptoJS.PBKDF2(password, salt, {
                        keySize: 256/32,
                        iterations: 1000
                    });
                    
                    // Decrypt using AES-GCM
                    const decrypted = CryptoJS.AES.decrypt({
                        ciphertext: ct
                    }, key, {
                        iv: iv,
                        mode: CryptoJS.mode.GCM,
                        padding: CryptoJS.pad.Pkcs7
                    });
                    
                    return decrypted.toString(CryptoJS.enc.Utf8);
                } catch (error) {
                    console.error('Decryption error:', error);
                    throw new Error('Decryption failed - incorrect password or corrupted data');
                }
            }
        }

        // =======================================================
        // TEXTMOJI ENCRYPTION SYSTEM WITH AES-256-GCM
        // =======================================================
        
        class SecureEmojiEncryption {
            constructor() {
                this.aes = new AES256GCM();
                this.emojiSet = [
                    '😀', '😃', '😄', '😁', '😆', '😅', '😂', '🤣', '😊', '😇', '🙂', '🙃', '😉', '😌', '😍', '🥰',
                    '😘', '😗', '😙', '😚', '😋', '😛', '😝', '😜', '🤪', '🤨', '🧐', '🤓', '😎', '🤩', '🥳', '😏',
                    '😒', '😞', '😔', '😟', '😕', '🙁', '☹️', '😣', '😖', '😫', '😩', '🥺', '😢', '😭', '😤', '😠',
                    '😡', '🤬', '🤯', '😳', '🥵', '🥶', '😱', '😨', '😰', '😥', '😓', '🤗', '🤔', '🤭', '🤫', '🤥',
                    '😶', '😐', '😑', '😬', '🙄', '😯', '😦', '😧', '😮', '😲', '🥱', '😴', '🤤', '😪', '😵', '🤐',
                    '🥴', '🤢', '🤮', '🤧', '😷', '🤒', '🤕', '🤑', '🤠', '😈', '👿', '👹', '👺', '🤡', '💩', '👻',
                    '💀', '☠️', '👽', '👾', '🤖', '🎃', '😺', '😸', '😹', '😻', '😼', '😽', '🙀', '😿', '😾', '🙈',
                    '🙉', '🙊', '💋', '💌', '💘', '💝', '💖', '💗', '💓', '💞', '💕', '💟', '❣️', '💔', '❤️', '🧡',
                    '💛', '💚', '💙', '💜', '🖤', '💯', '💢', '💥', '💫', '💦', '💨', '🕳️', '💣', '💬', '👁️‍🗨️', '🗨️', '🗯️',
                    '💭', '💤', '👋', '🤚', '🖐️', '✋', '🖖', '👌', '🤏', '✌️', '🤞', '🤟', '🤘', '🤙', '👈', '👉',
                    '👆', '🖕', '👇', '☝️', '👍', '👎', '✊', '👊', '🤛', '🤜', '👏', '🙌', '👐', '🤲', '🤝', '🙏',
                    '✍️', '💅', '🤳', '💪', '🦾', '🦿', '🦵', '🦶', '👂', '🦻', '👃', '🧠', '🦷', '🦴', '👀', '👁️',
                    '👅', '👄', '👶', '🧒', '👦', '👧', '🧑', '👱', '👨', '🧔', '👨‍🦰', '👨‍🦱', '👨‍🦳', '👨‍🦲', '👩', '👩‍🦰',
                    '👩‍🦱', '👩‍🦳', '👩‍🦲', '🧓', '👴', '👵', '🙍', '🙎', '🙅', '🙆', '💁', '🙋', '🧏', '🙇', '🤦', '🤷',
                    '👮', '🕵️', '💂', '👷', '🤴', '👸', '👳', '👲', '🧕', '🤵', '👰', '🤰', '🤱', '👼', '🎅', '🤶',
                    '🦸', '🦹', '🧙', '🧚', '🧛', '🧜', '🧝', '🧞', '🧟', '💆', '💇', '🚶', '🧍', '🧎', '🏃', '💃',
                    '🕺', '🕴️', '👯', '🧖', '🧗', '🤺', '🏇', '⛷️', '🏂', '🏌️', '🏄', '🚣', '🏊', '⛹️', '🏋️', '🚴',
                    '🚵', '🤸', '🤼', '🤽', '🤾', '🤹', '🧘', '🛀', '🛌', '👭', '👫', '👬', '💏', '💑', '👪', '🗣️',
                    '👤', '👥', '🫂', '👣', '🦰', '🦱', '🦳', '🦲', '🐵', '🐒', '🦍', '🦧', '🐶', '🐕', '🦮', '🐩',
                    '🐺', '🦊', '🦝', '🐱', '🐈', '🦁', '🐯', '🐅', '🐆', '🐴', '🐎', '🦄', '🦓', '🦌', '🐮', '🐂',
                    '🐃', '🐄', '🐷', '🐖', '🐗', '🐽', '🐏', '🐑', '🐐', '🐪', '🐫', '🦙', '🦒', '🐘', '🦣', '🦏',
                    '🦛', '🐭', '🐁', '🐀', '🐹', '🐰', '🐇', '🐿️', '🦫', '🦔', '🦇', '🐻', '🐨', '🐼', '🦥', '🦦',
                    '🦨', '🦘', '🦡', '🐾', '🦃', '🐔', '🐓', '🐣', '🐤', '🐥', '🐦', '🐧', '🕊️', '🦅', '🦆', '🦢',
                    '🦉', '🦤', '🪶', '🦩', '🦚', '🦜', '🐸', '🐊', '🐢', '🦎', '🐍', '🐲', '🐉', '🦕', '🦖', '🐳',
                    '🐋', '🐬', '🦭', '🐟', '🐠', '🐡', '🦈', '🐙', '🐚', '🪸', '🐌', '🦋', '🐛', '🐜', '🐝', '🪲',
                    '🐞', '🦗', '🪳', '🕷️', '🕸️', '🦂', '🦟', '🪰', '🪱', '🦠', '💐', '🌸', '💮', '🏵️', '🌹', '🥀',
                    '🌺', '🌻', '🌼', '🌷', '🌱', '🪴', '🌲', '🌳', '🌴', '🌵', '🌾', '🌿', '☘️', '🍀', '🍁', '🍂',
                    '🍃', '🍄', '🍇', '🍈', '🍉', '🍊', '🍋', '🍌', '🍍', '🥭', '🍎', '🍏', '🍐', '🍑', '🍒', '🍓',
                    '🫐', '🥝', '🍅', '🫒', '🥥', '🥑', '🍆', '🥔', '🥕', '🌽', '🌶️', '🫑', '🥒', '🥬', '🥦', '🧄',
                    '🧅', '🍄', '🥜', '🫘', '🌰', '🍞', '🥐', '🥖', '🫓', '🥨', '🥯', '🥞', '🧇', '🧀', '🍖', '🍗',
                    '🥩', '🥓', '🍔', '🍟', '🍕', '🌭', '🥪', '🌮', '🌯', '🫔', '🥙', '🧆', '🥚', '🍳', '🥘', '🍲',
                    '🫕', '🥣', '🥗', '🍿', '🧈', '🧂', '🥫', '🍱', '🍘', '🍙', '🍚', '🍛', '🍜', '🍝', '🍠', '🍢',
                    '🍣', '🍤', '🍥', '🥮', '🍡', '🥟', '🥠', '🥡', '🦀', '🦞', '🦐', '🦑', '🦪', '🍦', '🍧', '🍨',
                    '🍩', '🍪', '🎂', '🍰', '🧁', '🥧', '🍫', '🍬', '🍭', '🍮', '🍯', '🍼', '🥛', '☕', '🫖', '🍵',
                    '🍶', '🍾', '🍷', '🍸', '🍹', '🍺', '🍻', '🥂', '🥃', '🥤', '🧋', '🧃', '🧉', '🧊', '🥢', '🍽️',
                    '🍴', '🥄', '🔪', '🏺', '🌍', '🌎', '🌏', '🌐', '🗺️', '🗾', '🧭', '🏔️', '⛰️', '🌋', '🗻', '🏕️',
                    '🏖️', '🏜️', '🏝️', '🏞️', '🏟️', '🏛️', '🏗️', '🧱', '🪨', '🪵', '🛖', '🏘️', '🏚️', '🏠', '🏡',
                    '🏢', '🏣', '🏤', '🏥', '🏦', '🏨', '🏩', '🏪', '🏫', '🏬', '🏭', '🏯', '🏰', '💒', '🗼', '🗽',
                    '⛪', '🕌', '🛕', '🕍', '⛩️', '🕋', '⛲', '⛺', '🌁', '🌃', '🏙️', '🌄', '🌅', '🌆', '🌇', '🌉',
                    '♨️', '🎠', '🎡', '🎢', '💈', '🎪', '🚂', '🚃', '🚄', '🚅', '🚆', '🚇', '🚈', '🚉', '🚊', '🚝',
                    '🚞', '🚋', '🚌', '🚍', '🚎', '🚐', '🚑', '🚒', '🚓', '🚔', '🚕', '🚖', '🚗', '🚘', '🚙', '🛻',
                    '🚚', '🚛', '🚜', '🏎️', '🏍️', '🛵', '🚲', '🛴', '🛹', '🛼', '🚏', '🛣️', '🛤️', '🛢️', '⛽', '🚨',
                    '🚥', '🚦', '🛑', '🚧', '⚓', '⛵', '🛶', '🚤', '🛳️', '⛴️', '🛥️', '🚢', '✈️', '🛩️', '🛫', '🛬',
                    '🪂', '💺', '🚁', '🚟', '🚠', '🚡', '🛰️', '🚀', '🛸', '🛎️', '🧳', '⌛', '⏳', '⌚', '⏰', '⏱️', '⏲️',
                    '🕰️', '🌡️', '⛱️', '🧨', '🎈', '🎉', '🎊', '🎎', '🎏', '🎐', '🎀', '🎁', '🤿', '🪀', '🪁', '🔮',
                    '🪄', '🧿', '🕹️', '🧩', '🧸', '🪅', '🪆', '♠️', '♥️', '♦️', '♣️', '♟️', '🃏', '🀄', '🎴', '🎭',
                    '🖼️', '🎨', '🧵', '🪡', '🧶', '🪢', '👓', '🕶️', '🥽', '🥼', '🦺', '👔', '👕', '👖', '🧣', '🧤',
                    '🧥', '🧦', '👗', '👘', '🥻', '🩱', '🩲', '🩳', '👙', '👚', '👛', '👜', '👝', '🛍️', '🎒', '👞',
                    '👟', '🥾', '🥿', '👠', '👡', '🩰', '👢', '👑', '👒', '🎩', '🎓', '🧢', '🪖', '⛑️', '💄', '💍',
                    '💼', '🩴'
                ];
            }

            generateEmojiMapping(password) {
                const hash = CryptoJS.SHA256(password).toString();
                const mapping = {};
                
                // Create a copy of the emoji set and shuffle it
                const shuffledEmojis = [...this.emojiSet];
                for (let i = shuffledEmojis.length - 1; i > 0; i--) {
                    const hashIndex = parseInt(hash.substr(i * 2, 2), 16) % (i + 1);
                    [shuffledEmojis[i], shuffledEmojis[hashIndex]] = [shuffledEmojis[hashIndex], shuffledEmojis[i]];
                }
                
                // Create mapping for all 256 possible byte values
                for (let i = 0; i < 256; i++) {
                    mapping[i] = shuffledEmojis[i % shuffledEmojis.length];
                }
                
                return mapping;
            }

            async encrypt(text, password) {
                try {
                    // First encrypt with AES-256-GCM
                    const encryptedData = await this.aes.encrypt(text, password);
                    
                    // Convert the encrypted result to emojis
                    const emojiMapping = this.generateEmojiMapping(password);
                    let emojiString = '';
                    
                    // Convert each character to emoji
                    for (let i = 0; i < encryptedData.length; i++) {
                        const charCode = encryptedData.charCodeAt(i);
                        emojiString += emojiMapping[charCode % 256];
                    }
                    
                    return {
                        emojis: emojiString,
                        data: encryptedData
                    };
                } catch (error) {
                    console.error('Encryption error:', error);
                    throw new Error('Encryption failed');
                }
            }

            async decrypt(emojiString, password) {
                try {
                    const emojiMapping = this.generateEmojiMapping(password);
                    const reverseMapping = {};
                    
                    for (const [byte, emoji] of Object.entries(emojiMapping)) {
                        reverseMapping[emoji] = parseInt(byte);
                    }
                    
                    let encryptedData = '';
                    const emojiArray = emojiString.match(/[\uD800-\uDBFF][\uDC00-\uDFFF]|./gs) || [];
                    
                    for (const emoji of emojiArray) {
                        if (reverseMapping[emoji] !== undefined) {
                            encryptedData += String.fromCharCode(reverseMapping[emoji]);
                        }
                    }
                    
                    const decryptedText = await this.aes.decrypt(encryptedData, password);
                    
                    return decryptedText;
                } catch (error) {
                    console.error('Decryption error:', error);
                    throw new Error('Decryption failed - incorrect password or corrupted data');
                }
            }
        }

        // Initialize encryption system
        const encryptionSystem = new SecureEmojiEncryption();

        // =======================================================
        // DASHBOARD FUNCTIONALITY - FIXED
        // =======================================================

        // History tab verification
        document.getElementById('history-tab').addEventListener('click', (e) => {
            e.preventDefault();
            
            // Show verification modal
            document.getElementById('verify-modal').style.display = 'flex';
        });
        
        // Settings modal
        document.getElementById('settings-tab').addEventListener('click', (e) => {
            e.preventDefault();
            document.getElementById('settings-modal').style.display = 'flex';
        });
        
        document.getElementById('close-settings').addEventListener('click', () => {
            document.getElementById('settings-modal').style.display = 'none';
        });
        
        // Verification modal
        document.getElementById('close-verify').addEventListener('click', () => {
            document.getElementById('verify-modal').style.display = 'none';
        });
        
        document.getElementById('verify-submit').addEventListener('click', () => {
            const username = document.getElementById('verify-username').value.trim();
            const password = document.getElementById('verify-password').value;
            const errorElement = document.getElementById('verify-error');
            
            if (!username || !password) {
                errorElement.textContent = 'Please enter both username and password';
                errorElement.style.display = 'block';
                return;
            }
            
            const users = JSON.parse(localStorage.getItem('textmoji_users')) || {};
            const user = users[username];
            
            if (user && user.password === simpleHash(password)) {
                // Verification successful
                document.getElementById('verify-modal').style.display = 'none';
                
                // Update navigation
                document.querySelectorAll('.nav-item').forEach(i => {
                    i.classList.remove('active');
                });
                document.getElementById('history-tab').classList.add('active');
                
                // Show history
                document.getElementById('page-title').textContent = 'History';
                document.getElementById('encryption-card').style.display = 'none';
                document.getElementById('decryption-card').style.display = 'none';
                document.getElementById('history-card').style.display = 'block';
                
                loadHistory();
            } else {
                errorElement.textContent = 'Invalid username or password';
                errorElement.style.display = 'block';
            }
        });
        
        // Logout
        document.getElementById('logout-btn').addEventListener('click', (e) => {
            e.preventDefault();
            logout();
        });
        
        // Theme toggle
        document.getElementById('theme-toggle').addEventListener('click', () => {
            document.body.classList.toggle('light-theme');
            const icon = document.querySelector('#theme-toggle i');
            if (document.body.classList.contains('light-theme')) {
                icon.classList.remove('fa-moon');
                icon.classList.add('fa-sun');
            } else {
                icon.classList.remove('fa-sun');
                icon.classList.add('fa-moon');
            }
        });
        
        // Menu toggle for mobile
        document.getElementById('menu-toggle').addEventListener('click', () => {
            document.querySelector('.sidebar').classList.toggle('active');
        });
        
        // Toggle password visibility
        document.querySelectorAll('.toggle-password').forEach(button => {
            button.addEventListener('click', function() {
                const input = this.parentElement.querySelector('input');
                const icon = this.querySelector('i');
                
                if (input.type === 'password') {
                    input.type = 'text';
                    icon.classList.remove('fa-eye');
                    icon.classList.add('fa-eye-slash');
                } else {
                    input.type = 'password';
                    icon.classList.remove('fa-eye-slash');
                    icon.classList.add('fa-eye');
                }
            });
        });
        
        // Encryption functionality
        document.getElementById('encrypt-btn').addEventListener('click', async () => {
            const inputText = document.getElementById('textmsg').value.trim();
            const pass = document.getElementById('password').value.trim();
            const feedback = document.getElementById('encrypt-password-feedback');
            
            if (!inputText) {
                showError('Please enter a message to encrypt');
                return;
            }
            
            if (!pass) {
                showError('Please set an encryption password');
                feedback.textContent = 'Password is required for encryption';
                feedback.className = 'password-feedback error';
                feedback.style.display = 'block';
                return;
            }
            
            if (pass.length < 8) {
                showError('Encryption password must be at least 8 characters');
                feedback.textContent = 'Password must be at least 8 characters';
                feedback.className = 'password-feedback error';
                feedback.style.display = 'block';
                return;
            }
            
            feedback.style.display = 'none';
            
            try {
                const result = await encryptionSystem.encrypt(inputText, pass);
                
                document.getElementById('encrypted-output').textContent = result.emojis;
                document.getElementById('encrypt-result').style.display = 'block';
                
                // Save to user's encryption history
                const username = localStorage.getItem('currentUser');
                if (username) {
                    const users = JSON.parse(localStorage.getItem('textmoji_users'));
                    if (users && users[username]) {
                        if (!users[username].data) users[username].data = {};
                        if (!users[username].data.encryptionHistory) users[username].data.encryptionHistory = [];
                        
                        users[username].data.encryptionHistory.push({
                            type: 'encryption',
                            message: inputText,
                            result: result.emojis,
                            timestamp: new Date().toISOString()
                        });
                        
                        localStorage.setItem('textmoji_users', JSON.stringify(users));
                    }
                }
                
                showSuccess('Message encrypted successfully!');
            } catch (error) {
                showError('Encryption failed: ' + error.message);
            }
        });
        
        // Decryption functionality
        document.getElementById('decrypt-btn').addEventListener('click', async () => {
            const inputEmoji = document.getElementById('emojimsg').value.trim();
            const pass = document.getElementById('finalpassword').value.trim();
            const feedback = document.getElementById('decrypt-password-feedback');
            
            if (!inputEmoji) {
                showError('Please enter emojis to decrypt');
                return;
            }
            
            if (!pass) {
                showError('Please enter the decryption password');
                feedback.textContent = 'Password is required for decryption';
                feedback.className = 'password-feedback error';
                feedback.style.display = 'block';
                return;
            }
            
            feedback.style.display = 'none';
            
            try {
                const decryptedText = await encryptionSystem.decrypt(inputEmoji, pass);
                
                if (!decryptedText) {
                    throw new Error('Decryption resulted in empty text');
                }
                
                document.getElementById('decrypted-output').textContent = decryptedText;
                document.getElementById('decrypt-result').style.display = 'block';
                
                // Save to user's encryption history
                const username = localStorage.getItem('currentUser');
                if (username) {
                    const users = JSON.parse(localStorage.getItem('textmoji_users'));
                    if (users && users[username]) {
                        if (!users[username].data) users[username].data = {};
                        if (!users[username].data.encryptionHistory) users[username].data.encryptionHistory = [];
                        
                        users[username].data.encryptionHistory.push({
                            type: 'decryption',
                            message: inputEmoji,
                            result: decryptedText,
                            timestamp: new Date().toISOString()
                        });
                        
                        localStorage.setItem('textmoji_users', JSON.stringify(users));
                    }
                }
                
                showSuccess('Message decrypted successfully!');
            } catch (error) {
                showError('Decryption failed: ' + error.message);
                feedback.textContent = 'Incorrect password or corrupted data';
                feedback.className = 'password-feedback error';
                feedback.style.display = 'block';
                
                document.getElementById('decrypted-output').textContent = '';
                document.getElementById('decrypt-result').style.display = 'none';
            }
        });
        
        // Copy buttons
        document.getElementById('copy-encrypted').addEventListener('click', () => {
            const textToCopy = document.getElementById('encrypted-output').textContent;
            copyToClipboard(textToCopy);
            showSuccess('Encrypted message copied to clipboard!');
        });
        
        document.getElementById('copy-decrypted').addEventListener('click', () => {
            const textToCopy = document.getElementById('decrypted-output').textContent;
            copyToClipboard(textToCopy);
            showSuccess('Decrypted message copied to clipboard!');
        });
        
        // Utility functions
        function copyToClipboard(text) {
            const textarea = document.createElement('textarea');
            textarea.value = text;
            document.body.appendChild(textarea);
            textarea.select();
            document.execCommand('copy');
            document.body.removeChild(textarea);
        }
        
        function showSuccess(message) {
            const alert = document.getElementById('success-alert');
            document.getElementById('success-message').textContent = message;
            alert.style.display = 'block';
            document.getElementById('error-alert').style.display = 'none';
            
            setTimeout(() => {
                alert.style.display = 'none';
            }, 5000);
        }
        
        function showError(message) {
            const alert = document.getElementById('error-alert');
            document.getElementById('error-message').textContent = message;
            alert.style.display = 'block';
            document.getElementById('success-alert').style.display = 'none';
            
            setTimeout(() => {
                alert.style.display = 'none';
            }, 5000);
        }
        
        // Load history
        function loadHistory() {
            const username = localStorage.getItem('currentUser');
            if (!username) return;
            
            const users = JSON.parse(localStorage.getItem('textmoji_users'));
            const user = users[username];
            const historyBody = document.getElementById('history-body');
            historyBody.innerHTML = '';
            
            if (!user || !user.data || !user.data.encryptionHistory || user.data.encryptionHistory.length === 0) {
                historyBody.innerHTML = '<tr><td colspan="4" style="text-align: center; color: var(--gray);">No history yet</td></tr>';
                return;
            }
            
            user.data.encryptionHistory.slice(-10).reverse().forEach(item => {
                const row = document.createElement('tr');
                
                const date = new Date(item.timestamp);
                const formattedDate = `${date.toLocaleDateString()} ${date.toLocaleTimeString()}`;
                
                row.innerHTML = `
                    <td>${formattedDate}</td>
                    <td>${item.type === 'encryption' ? 'Encryption' : 'Decryption'}</td>
                    <td>${item.message.substring(0, 30)}${item.message.length > 30 ? '...' : ''}</td>
                    <td>Success</td>
                `;
                
                historyBody.appendChild(row);
            });
        }
        
        // Check if user is already logged in
        window.onload = function() {
            const currentUser = localStorage.getItem('currentUser');
            if (currentUser) {
                showAppPage();
                
                // Load user specific data
                const users = JSON.parse(localStorage.getItem('textmoji_users'));
                const user = users[currentUser];
                
                if (user && user.data && user.data.encryptionHistory) {
                    localStorage.setItem('encryptionHistory', JSON.stringify(user.data.encryptionHistory));
                }
            } else {
                showLoginPage();
            }
        };
    </script>
</body>
</html>
