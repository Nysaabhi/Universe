<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Deep Chatbot - On-Demand Services</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/flatpickr/4.6.13/flatpickr.min.css" rel="stylesheet">
    <style>
        :root {
            --primary-color: #FFD700;
            --secondary-color: #FDB931;
            --background-dark: #1a1a1f;
            --text-light: #ffffff;
            --text-dark: #000000;
            --accent-gradient: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
            --error-color: #ff4444;
            --success-color: #00C851;
        }
        
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            background-color: var(--background-dark);
            color: var(--text-light);
            line-height: 1.6;
            min-height: 100vh;
            overflow-x: hidden;
        }
        
        .header {
            padding: 8px 16px;
            background: rgba(26, 26, 31, 0.95);
            position: fixed;
            width: 100%;
            top: 0;
            left: 0;
            z-index: 30;
            border-bottom: 2px solid var(--primary-color);
            backdrop-filter: blur(10px);
            height: 60px;
        }
        
        .header-content {
            max-width: 1400px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            align-items: center;
            height: 100%;
        }
        
        .logo {
            font-size: 1.8em;
            font-weight: 900;
            background: var(--accent-gradient);
            -webkit-background-clip: text;
            color: transparent;
            letter-spacing: 1.2px;
        }
        
        .wallet {
            padding: 10px 20px;
            background: var(--accent-gradient);
            border: none;
            border-radius: 50px;
            color: var(--text-dark);
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 0.95em;
        }
        
        .chatbot-container {
            position: fixed;
            top: 60px;
            left: 0;
            right: 0;
            bottom: 60px;
            background: rgba(26, 26, 31, 0.95);
            display: flex;
            flex-direction: column;
        }
        
        .chat-header {
            padding: 15px;
            background: linear-gradient(135deg, rgba(255, 215, 0, 0.1), rgba(253, 185, 49, 0.1));
            border-bottom: 1px solid rgba(255, 215, 0, 0.2);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .chat-status {
            display: flex;
            align-items: center;
            color: var(--primary-color);
            font-weight: 600;
        }
        
        .status-dot {
            width: 8px;
            height: 8px;
            background: var(--primary-color);
            border-radius: 50%;
            margin-right: 8px;
            animation: pulse 2s infinite;
        }
        
        .chat-messages {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            scroll-behavior: smooth;
        }
        
        .message {
            display: flex;
            gap: 10px;
            max-width: 85%;
            margin-bottom: 16px;
            animation: messageSlide 0.3s ease-out;
        }
        
        .bot-message {
            align-self: flex-start;
        }
        
        .user-message {
            align-self: flex-end;
            flex-direction: row-reverse;
        }
        
        .message-avatar {
            width: 36px;
            height: 36px;
            min-width: 36px;
            min-height: 36px;
            background: rgba(255, 215, 0, 0.1);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--primary-color);
            flex-shrink: 0; /* Prevents shrinking */
        }
        
        .message {
            display: flex;
            gap: 10px;
            max-width: 90%; /* Increased for better mobile visibility */
            margin-bottom: 16px;
            animation: messageSlide 0.3s ease-out;
            align-items: flex-start; /* Aligns avatar with message top */
        }
        
        .message-content {
            background: rgba(255, 255, 255, 0.05);
            padding: 12px 16px;
            border-radius: 16px;
            color: var(--text-light);
        }
        
        .user-message .message-content {
            background: rgba(255, 215, 0, 0.1);
        }
        
        .chat-input-container {
            padding: 20px;
            background: rgba(26, 26, 31, 0.98);
            border-top: 1px solid rgba(255, 215, 0, 0.1);
            display: flex;
            gap: 12px;
            align-items: center;
        }
        
        #chatInput {
            flex: 1;
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 215, 0, 0.2);
            border-radius: 12px;
            padding: 20px 20px;
            color: var(--text-light);
            font-size: 0.95em;
        }
        
        .input-actions {
            display: flex;
            gap: 8px;
        }
         
        .input-actions button {
             padding: 20px 20px;
             background: var(--accent-gradient);
             border: none;
             border-radius: 8px;
             color: #ffffff;
             cursor: pointer;
        }
        
        .send-message i {
            font-size: 1.5em; /* Increase the icon size */
        }
        
        .alert {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 20px;
            border-radius: 8px;
            color: var(--text-light);
            z-index: 1000;
            animation: slideIn 0.3s ease-out;
        }
        
        .alert-success {
            background: var(--success-color);
        }
        
        .alert-error {
            background: var(--error-color);
        }
        
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            height: 60px;
            background: rgba(26, 26, 31, 0.98);
            padding: 8px 0;
            border-top: 1px solid rgba(255, 215, 0, 0.2);
        }
        
        .nav-container {
            display: flex;
            justify-content: space-around;
            align-items: center;
            height: 100%;
            max-width: 600px;
            margin: 0 auto;
        }
        
        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            text-decoration: none;
            color: #fff;
            transition: all 0.3s ease;
            padding: 5px;
            cursor: pointer;
        }
        
        .nav-item i {
            color: #fff;
            font-size: 20px;
            margin-bottom: 4px;
        }
        
        .nav-item span {
            font-size: 12px;
        }
        
        /* Animations */
        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }
        
        @keyframes messageSlide {
            from {
                opacity: 0;
                transform: translateY(10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateX(100px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }
        
        /* Responsive Design */
        @media (max-width: 768px) {
            .message {
                max-width: 90%;
            }
        }
        
        /* Hide default headings */
        body > h1:first-of-type:not(.heading) {
            display: none !important;
        }
        
        .markdown-body h1:first-child {
            display: none !important;
        }
        
        .position-relative h1:first-child {
            display: none !important;
        }
         
        .category-carousel {
            display: flex;
            width: 100%;
            overflow-x: auto;
            scroll-snap-type: x mandatory;
            scroll-behavior: smooth;
            -webkit-overflow-scrolling: touch;
            padding: 20px;
            gap: 15px;
            /* Hide scrollbar but keep functionality */
            scrollbar-width: none;  /* Firefox */
            -ms-overflow-style: none;  /* IE and Edge */
        }
        
        /* Hide scrollbar for Chrome, Safari and Opera */
        .category-carousel::-webkit-scrollbar {
            display: none;
        }
        
        .category-card {
            flex: 0 0 calc(25% - 15px); /* Account for gap */
            max-width: 1400px;
            scroll-snap-align: start;
            border: 1px solid rgba(255, 215, 0, 0.2);
            background: rgba(255, 215, 0, 0.1);
            border-radius: 12px;
            padding: 20px;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            text-align: center;
        }
        
        .category-card:hover {
            transform: translateY(-2px);
            background: rgba(255, 215, 0, 0.2);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
        
        .category-icon {
            font-size: 2em;
            color: var(--primary-color);
            margin-bottom: 10px;
        }
        
                .page-grid {
                    display: grid;
                    grid-template-columns: repeat(4, 1fr);
                    gap: 15px;
                    padding: 20px;
                }
        
                .package-grid {
                    display: none;
                    flex-direction: column;
                    gap: 15px;
                    padding: 20px;
                }
        
                .package-card {
                    background: rgba(255, 215, 0, 0.1);
                    border-radius: 12px;
                    padding: 20px;
                }
        
                .package-header {
                    display: flex;
                    justify-content: space-between;
                    align-items: center;
                    margin-bottom: 15px;
                }
        
                .package-title {
                    font-size: 1.2em;
                    font-weight: 600;
                    color: var(--primary-color);
                }
        
                .package-price {
                    font-size: 1.1em;
                    color: var(--text-light);
                }
        
                .package-features {
                    margin: 15px 0;
                }
        
                .package-actions {
                    display: flex;
                    gap: 10px;
                    margin-top: 15px;
                }
        
                .action-button {
                    flex: 1;
                    padding: 10px;
                    border: none;
                    border-radius: 8px;
                    cursor: pointer;
                    font-weight: 600;
                    transition: all 0.3s ease;
                }
        
                .book-button {
                    background: var(--accent-gradient);
                    color: var(--text-dark);
                }
        
                .call-button {
                    background: rgba(255, 255, 255, 0.1);
                    color: var(--text-light);
                }
        
                .info-button {
                background: none;
                border: none;
                color: var(--primary-color);
                cursor: pointer;
                padding: 0 5px;
                font-size: 1.1em;
                transition: color 0.3s ease;
            }
            
            .info-button:hover {
                color: #ffffff;
            }
            
            /* Add these styles to your CSS */
        
        #packageInfoModal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1000;
        }
        
        .modal-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .modal-content {
            background-color: #1a1a1f;
            border-radius: 12px;
            width: 100%;
            max-width: 600px;
            max-height: 90vh;
            overflow-y: auto;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
        }
        
        .modal-header {
            padding: 20px;
            border-bottom: 1px solid #eee;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .modal-header h2 {
            margin: 0;
            font-size: 1.5rem;
            color: #fffdfd;
        }
        
        .close-button {
            background: none;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
            color: #b1b1b1;
            padding: 5px;
        }
        
        .modal-body {
            padding: 20px;
        }
        
        .package-price-section {
            text-align: center;
            margin-bottom: 30px;
        }
        
        .price-tag {
            font-size: 2.5rem;
            font-weight: bold;
            color: #ffffff;
        }
        
        .price-duration {
            color: #ffffff;
            font-size: 0.9rem;
        }
        
        .package-details-section {
            margin-bottom: 30px;
        }
        
        .feature-list {
            list-style: none;
            padding: 0;
            margin: 15px 0;
        }
        
        .feature-list li {
            padding: 10px 0;
            display: flex;
            align-items: center;
            color: #ffffff;
        }
        
        .feature-list li i {
            color: #4CAF50;
            margin-right: 10px;
        }
        
        .benefits-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }
        
        .benefit-item {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 10px;
            background-color: #1a1a1f;
            border-radius: 8px;
        }
        
        .benefit-item i {
            color: #FFD700;
            font-size: 1.2rem;
        }
        
        .modal-footer {
            padding: 20px;
            border-top: 1px solid #eee;
            display: flex;
            gap: 10px;
            justify-content: flex-end;
        }
        
        .modal-footer .action-button {
            padding: 12px 24px;
            border-radius: 6px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .modal-footer .book-button {
            background-color: #4CAF50;
            color: white;
            border: none;
        }
        
        .modal-footer .call-button {
            background-color: #f8f9fa;
            color: #333;
            border: 1px solid #ddd;
        }
        
        .modal-footer .action-button:hover {
            transform: translateY(-1px);
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        }
        
                .booking-form {
                    display: none;
                    padding: 20px;
                    background: rgba(26, 26, 31, 0.98);
                    position: fixed;
                    bottom: 60px;
                    left: 0;
                    right: 0;
                    z-index: 20;
                    border-top: 1px solid rgba(255, 215, 0, 0.2);
                    animation: slideUp 0.3s ease-out;
                }
        
                .form-group {
                    margin-bottom: 15px;
                }
        
                .form-group label {
                    display: block;
                    margin-bottom: 5px;
                    color: var(--primary-color);
                }
        
                .form-group input {
                    width: 100%;
                    padding: 10px;
                    background: rgba(255, 255, 255, 0.05);
                    border: 1px solid rgba(255, 215, 0, 0.2);
                    border-radius: 8px;
                    color: var(--text-light);
                }
        
                .form-row {
            display: flex;
            justify-content: space-between;
            gap: 10px; /* Adjust the spacing between elements as needed */
        }
        
        .form-row .form-group {
            flex: 1;
        }
        
                .submit-booking {
                    width: 100%;
                    padding: 12px;
                    background: var(--accent-gradient);
                    border: none;
                    border-radius: 8px;
                    color: var(--text-dark);
                    font-weight: 600;
                    cursor: pointer;
                }
        
                .menu-button {
                    background: none;
                    border: none;
                    color: var(--primary-color);
                    cursor: pointer;
                    padding: 10px;
                }
        
                .menu-overlay {
            display: none;
            position: fixed;
            top: 60px;
            left: 0;
            right: 0;
            bottom: 60px;
            background: rgba(26, 26, 31, 0.98);
            z-index: 25;
            padding: 0;
            animation: fadeIn 0.3s ease-out;
            overflow-y: auto;
        }

        .menu-sections {
            padding: 20px;
        }

        .menu-section {
            margin-bottom: 24px;
        }

        .menu-section-title {
            color: var(--primary-color);
            font-size: 1.2em;
            font-weight: 600;
            margin-bottom: 12px;
            padding-bottom: 8px;
            border-bottom: 1px solid rgba(255, 215, 0, 0.2);
        }

        .menu-items {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .menu-item {
            padding: 12px 16px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 8px;
            color: var(--text-light);
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .menu-item:hover {
            background: rgba(255, 215, 0, 0.1);
        }

        .menu-item-price {
            color: var(--primary-color);
            font-weight: 600;
        }

        .social-links {
            display: flex;
            gap: 12px;
        }

        .social-link {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.05);
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--text-light);
            text-decoration: none;
            transition: all 0.3s ease;
        }

        .social-link:hover {
            background: var(--primary-color);
            color: var(--text-dark);
        }

        .about-content {
            line-height: 1.6;
            color: #cccccc;
        }

        /* Animation for menu items */
        .menu-item {
            animation: slideIn 0.3s ease-out;
            animation-fill-mode: both;
        }

        .menu-item:nth-child(1) { animation-delay: 0.1s; }
        .menu-item:nth-child(2) { animation-delay: 0.2s; }
        .menu-item:nth-child(3) { animation-delay: 0.3s; }
        .menu-item:nth-child(4) { animation-delay: 0.4s; }
        .menu-item:nth-child(5) { animation-delay: 0.5s; }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateX(-20px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }
        
                @media (max-width: 768px) {
                    .category-card {
                        flex: 0 0 50%;
                    }
                    .page-grid {
                        grid-template-columns: repeat(2, 1fr);
                    }
                }
        
                .whatsapp-item {
            background-color: #25D366; /* WhatsApp green */
            color: white; /* Text color */
            border-radius: 8px; /* Optional: Rounded corners */
            padding: 10px; /* Optional: Add some padding */
            text-align: center;
        }
        
        /* Adjust the icon color */
        .whatsapp-item a i {
            color: white; /* WhatsApp icon color */
        }
        
        /* Optional: Change the hover effect */
        .whatsapp-item:hover {
            background-color: #1ebe57; /* Darker green on hover */
            color: white;
        }
        
         </style>
        </head>
<body>
    <header class="header">
        <div class="header-content">
            <div class="logo">Deep</div>
            <button class="wallet">
                <i class="fas fa-wallet"></i>
                Wallet
            </button>
        </div>
    </header>

    <div class="chatbot-container">
        <div class="chat-header">
            <div class="chat-status">
                <span class="status-dot"></span>
                Deep AI Assistant
            </div>
            <button class="menu-button" id="menuButton">
                <i class="fas fa-bars"></i>
            </button>
        </div>

        <div class="chat-messages" id="chatMessages">
            <div class="category-grid" id="categoryGrid">
                <!-- Categories will be dynamically populated -->
            </div>

            <div class="package-grid" id="packageGrid">
                <!-- Packages will be dynamically populated -->
            </div>

            <div class="category-carousel" id="categoryCarousel">
                <!-- Dynamically populated category cards -->
            </div>
        
            <div class="page-grid" id="pageGrid" style="display: none;">
                <!-- Dynamically populated category pages -->
            </div>
        
        <div class="booking-form" id="bookingForm">
            <div class="form-group">
                <label>Name</label>
                <input type="text" id="nameInput" placeholder="Enter your name" />
            </div>
            <div class="form-group">
                <label>Contact</label>
                <input type="tel" id="contactInput" placeholder="Enter your contact number" />
            </div>
            <div class="form-group">
                <label>Address</label>
                <input type="text" id="addressInput" placeholder="Enter your address (optional)" />
            </div>
            <div class="form-group">
                <label>Pincode</label>
                <input type="text" id="pincodeInput" placeholder="Enter pincode (optional)" />
            </div>
            <div class="form-row">
                <div class="form-group">
                    <label>Date</label>
                    <input type="text" id="dateInput" placeholder="Select date" />
                </div>
                <div class="form-group">
                    <label>Time Slot</label>
                    <input type="text" id="timeInput" placeholder="Select time slot" />
                </div>
            </div>
                        <button class="submit-booking" id="submitBooking">Book Now</button>
        </div>
    </div>

    
    <!-- Bottom Navigation -->
    <nav class="bottom-nav">
        <div class="nav-container">
            <div class="nav-item" data-page="subscription">
                <a href="https://nysaabhi.github.io/chat">
                    <i class="fas fa-comments"></i>
                </a>
                <span>Feedback</span>
            </div>
            <div class="nav-item" data-page="email">
                <a href="mailto:services.deepchatbot@gmail.com">
                    <i class="fas fa-envelope"></i>
                </a>
                <span>Email</span>
            </div>
                        <div class="nav-item whatsapp-item" data-page="chat">
                <a href="https://wa.me/9111478356" target="_blank">
                    <i class="fas fa-message"></i>
                </a>
                <span>WhatsApp</span>
            </div>
                                    <div class="nav-item" data-page="location">
                <a href="location.html">
                    <i class="fas fa-map-marker-alt"></i>
                </a>
                <span>Location</span>
            </div>
            <div class="nav-item" data-page="listings">
                <a href="listings.html">
                    <i class="fas fa-tasks"></i>
                </a>
                <span>Partner</span>
            </div>
        </div>
    </nav>
    
    <div class="menu-overlay" id="menuOverlay">
        <div class="menu-sections">
            <div class="menu-section">
                <div class="menu-section-title">About Us</div>
                <div class="about-content">
                    Deep Chatbot is your premier destination for on-demand services. We connect you with skilled professionals to meet all your service needs with just a few taps.
                </div>
            </div>

            <div class="menu-section">
                <div class="menu-section-title">Our Services</div>
                <div class="menu-items">
                    <div class="menu-item" data-category="cleaning">
                        <span>Home Cleaning</span>
                        <span class="menu-item-price">$49/hr</span>
                    </div>
                    <div class="menu-item" data-category="plumbing">
                        <span>Plumbing Services</span>
                        <span class="menu-item-price">$65/hr</span>
                    </div>
                    <div class="menu-item" data-category="electrical">
                        <span>Electrical Work</span>
                        <span class="menu-item-price">$75/hr</span>
                    </div>
                    <div class="menu-item" data-category="painting">
                        <span>House Painting</span>
                        <span class="menu-item-price">$55/hr</span>
                    </div>
                </div>
            </div>

            <div class="menu-section">
                <div class="menu-section-title">Connect With Us</div>
                <div class="social-links">
                    <a href="https://nysaabhi.github.io/chat" class="social-link">
                        <i class="fab fa-facebook-f"></i>
                    </a>
                    <a href="#" class="social-link">
                        <i class="fab fa-twitter"></i>
                    </a>
                    <a href="#" class="social-link">
                        <i class="fab fa-instagram"></i>
                    </a>
                    <a href="#" class="social-link">
                        <i class="fab fa-linkedin-in"></i>
                    </a>
                </div>
            </div>
        </div>
    </div>

    <div class="chat-input-container">
        <input type="text" id="chatInput" placeholder="Search for services or ask a question..." />
        <div class="input-actions">
            <button class="send-message" id="sendButton">
                <i class="fas fa-paper-plane"></i>
            </button>
        </div>
    </div>



    <script src="https://cdnjs.cloudflare.com/ajax/libs/flatpickr/4.6.13/flatpickr.min.js"></script>
    <script>
// Configuration and Data
const categories = [
    { id: 'home', name: 'Home', icon: 'fa-home', 
      services: ['Plumbing', 'Electrical', 'Carpentry', 'Painting', 'Appliance Repair', 'AC/Heating Repair', 'Cleaning Services'] },
      { id: 'education', name: 'Education', icon: 'fa-graduation-cap',
      services: ['Haircut', 'Manicure', 'Pedicure', 'Massage', 'Facial'] },
      { id: 'healthcare', name: 'Healthcare', icon: 'fa-heartbeat' ,
     services: ['TV Repair', 'Phone Repair', 'Computer Repair', 'Furniture Repair'] },
     { id: 'hospitality', name: 'Hospitality', icon: 'fa-bed' ,
     services: ['Doctor Visit', 'Nursing', 'Physiotherapy', 'Lab Tests'] },
     { id: 'retail', name: 'Retail', icon: 'fa-shopping-cart' ,
    services: ['Tutoring', 'Language Classes', 'Test Prep', 'Skills Training'] },
    { id: 'manufacturing', name: 'Manufacturing', icon: 'fa-industry' ,
      services: ['Babysitting', 'Pet Grooming', 'Fitness Trainer'] },
      { id: 'co-working-space', name: 'Co-Working', icon: 'fa-users' ,
            services: ['Plumbing', 'Electrical', 'Carpentry', 'Painting', 'Appliance Repair', 'AC/Heating Repair', 'Cleaning Services'] },
    { id: 'corporate-office', name: 'Corporate', icon: 'fa-briefcase' ,
      services: ['Haircut', 'Manicure', 'Pedicure', 'Massage', 'Facial'] },
      { id: 'warehouse', name: 'Warehouse', icon: 'fa-boxes' ,
     services: ['TV Repair', 'Phone Repair', 'Computer Repair', 'Furniture Repair'] },
     { id: 'logistics', name: 'Logistics', icon: 'fa-truck' ,
     services: ['Doctor Visit', 'Nursing', 'Physiotherapy', 'Lab Tests'] },
     { id: 'residential', name: 'Residential', icon: 'fa-home' ,
     services: ['Tutoring', 'Language Classes', 'Test Prep', 'Skills Training'] },
     { id: 'fitness', name: 'Fitness', icon: 'fa-dumbbell' ,
      services: ['Babysitting', 'Pet Grooming', 'Fitness Trainer'] },
      { id: 'banks', name: 'Banks', icon: 'fa-university' ,
     services: ['Plumbing', 'Electrical', 'Carpentry', 'Painting', 'Appliance Repair', 'AC/Heating Repair', 'Cleaning Services'] },
     { id: 'restaurants', name: 'Restaurants', icon: 'fa-utensils' ,
    services: ['Haircut', 'Manicure', 'Pedicure', 'Massage', 'Facial'] },
    { id: 'energy-utilities', name: 'Energy', icon: 'fa-bolt' ,
     services: ['TV Repair', 'Phone Repair', 'Computer Repair', 'Furniture Repair'] },
     { id: 'entertainment', name: 'Entertainment', icon: 'fa-film' ,
     services: ['Doctor Visit', 'Nursing', 'Physiotherapy', 'Lab Tests'] },
     { id: 'agriculture', name: 'Agriculture', icon: 'fa-seedling' ,
    services: ['Tutoring', 'Language Classes', 'Test Prep', 'Skills Training'] },
    { id: 'government', name: 'Government', icon: 'fa-landmark' ,
      services: ['Babysitting', 'Pet Grooming', 'Fitness Trainer'] },
      { id: 'construction', name: 'Construction', icon: 'fa-hard-hat' ,
    services: ['Plumbing', 'Electrical', 'Carpentry', 'Painting', 'Appliance Repair', 'AC/Heating Repair', 'Cleaning Services'] },
    { id: 'shop', name: 'Shop',  icon: 'fa-store',
      services: ['Haircut', 'Manicure', 'Pedicure', 'Massage', 'Facial'] },
      { id: 'banquet-hall', name: 'Banquet Hall', icon: 'fa-building' ,
     services: ['TV Repair', 'Phone Repair', 'Computer Repair', 'Furniture Repair'] },
     { id: 'occasion', name: 'Occasion', icon: 'fa-calendar-alt',    
      services: ['Tutoring', 'Language Classes', 'Test Prep', 'Skills Training'] }
];

const servicePackages = {
    'Plumbing': {
        silver: {
            title: 'Basic Plumbing',
            price: '$49',
            features: ['Basic pipe repair', 'Leak detection', '1 Hour service', 'Standard tools']
        },
        gold: {
            title: 'Advanced Plumbing',
            price: '$99',
            features: ['Complex repairs', 'Drain cleaning', '2 Hour service', 'Professional tools', '24/7 support']
        },
        platinum: {
            title: 'Premium Plumbing',
            price: '$149',
            features: ['Complete plumbing solutions', 'Emergency service', 'Unlimited duration', 'Premium tools', 'Priority support']
        }
    },
    'default': {
        silver: {
            title: 'Silver Package',
            price: '$49',
            features: ['Basic service', '1 Hour duration', 'Standard tools', 'Phone support']
        },
        gold: {
            title: 'Gold Package',
            price: '$99',
            features: ['Premium service', '2 Hour duration', 'Professional tools', '24/7 support']
        },
        platinum: {
            title: 'Platinum Package',
            price: '$149',
            features: ['VIP service', 'Unlimited duration', 'Premium tools', 'Priority support']
        }
    }
};

// Enhanced Search Configuration with Q&A Support
const searchConfig = {
    services: {
        'plumbing': {
            keywords: ['pipe', 'leak', 'water', 'tap', 'faucet', 'drain', 'bathroom', 'sink', 'toilet', 'shower'],
            intents: [
                'water not coming',
                'pipe leaking',
                'tap broken',
                'bathroom flooding',
                'drain clogged',
                'plumber',
                'plumber in bhopal',
                'need plumber'
            ]
        },
        'appliance repair': {
            keywords: ['refrigerator', 'fridge', 'washing', 'machine', 'dishwasher', 'microwave', 'oven', 'appliance'],
            intents: [
                'fridge not working',
                'washing machine repair',
                'appliance broken',
                'need repair',
                'fix my appliance'
            ]
        }
    },
    qaDatabase: {
        'how do i book a service': {
            answer: 'To book a service, click on the category, select a service, choose a package, and fill out the booking form. You can also call us directly at +91 9669181789 for immediate assistance.',
            category: 'Booking'
        },
        'what are your working hours': {
            answer: 'Our service hours are from 9:00 AM to 6:00 PM daily. For emergency services, please call our support line.',
            category: 'General'
        },
        'do you offer emergency services': {
            answer: 'Yes, we offer emergency services for critical issues. Our Platinum packages include 24/7 emergency support with priority response.',
            category: 'Services'
        },
        'how can i contact support': {
            answer: 'You can contact our support team via email at services.deepchatbot@gmail.com or call us at +91 9111478356.',
            category: 'Support'
        },
        'what services do you offer': {
            answer: 'We offer a wide range of home and occasion services including Plumbing, Electrical, Carpentry, Painting, Appliance Repair, AC/Heating Repair, Cleaning Services, and Skills Training.',
            category: 'Services'
        }
    }
};

// Display Welcome Message Function
function displayWelcomeMessage() {
    const chatMessages = document.getElementById('chatMessages');
    const welcomeMessageDiv = document.createElement('div');
    welcomeMessageDiv.id = 'welcomeMessage';
    welcomeMessageDiv.className = 'message bot-message welcome-message';
    welcomeMessageDiv.innerHTML = `
        <div class="message-avatar">
            <i class="fas fa-robot"></i>
        </div>
        <div class="message-content">
            <p>Hello! I'm your On-Demand services assistant. How can I help you today?</p>
        </div>
    `;
    chatMessages.appendChild(welcomeMessageDiv);
    chatMessages.scrollTop = chatMessages.scrollHeight;
}

// Initialize components
document.addEventListener('DOMContentLoaded', function() {
    initializeCategoryCarousel();
    initializeDateTimePickers();
    setupEventListeners();
    initializeSearchFunctionality();
    
    // Display welcome message when page loads
    displayWelcomeMessage();
});

// Search Functionality
function initializeSearchFunctionality() {
    const chatInput = document.getElementById('chatInput');
    const sendButton = document.getElementById('sendButton');

    sendButton.addEventListener('click', handleSearch);
    chatInput.addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
            handleSearch();
        }
    });
}

// Enhanced Search Processing Function
function processSearch(query) {
    query = query.toLowerCase().trim();
    const results = {
        services: new Set(),
        qaAnswers: []
    };
    
    // Check for direct Q&A match
    const qaMatch = Object.entries(searchConfig.qaDatabase).find(([question, data]) => 
        calculateSimilarity(query, question.toLowerCase()) > 0.7
    );
    
    if (qaMatch) {
        results.qaAnswers.push({
            question: qaMatch[0],
            answer: qaMatch[1].answer,
            category: qaMatch[1].category
        });
        return results;
    }
    
    // Service name match
    Object.keys(searchConfig.services).forEach(service => {
        if (service.toLowerCase().includes(query)) {
            results.services.add(service);
        }
    });

    // Keyword matching for services
    Object.entries(searchConfig.services).forEach(([service, config]) => {
        if (config.keywords.some(keyword => query.includes(keyword.toLowerCase()))) {
            results.services.add(service);
        }
    });

    // Intent matching for services
    Object.entries(searchConfig.services).forEach(([service, config]) => {
        if (config.intents.some(intent => calculateSimilarity(query, intent.toLowerCase()) > 0.6)) {
            results.services.add(service);
        }
    });

    return results;
}

// Enhanced Similarity Calculation Function
function calculateSimilarity(str1, str2) {
    const set1 = new Set(str1.split(/\s+/));
    const set2 = new Set(str2.split(/\s+/));
    const intersection = new Set([...set1].filter(x => set2.has(x)));
    const union = new Set([...set1, ...set2]);
    return intersection.size / union.size;
}

function displayBotMessage(message) {
    const chatMessages = document.getElementById('chatMessages');
    const messageDiv = document.createElement('div');
    messageDiv.className = 'message bot-message';
    messageDiv.innerHTML = `
        <div class="message-avatar">
            <i class="fas fa-robot"></i>
        </div>
        <div class="message-content">
            <p>${message}</p>
        </div>
    `;
    chatMessages.appendChild(messageDiv);
    chatMessages.scrollTop = chatMessages.scrollHeight;
}

function displayUserMessage(message) {
    const chatMessages = document.getElementById('chatMessages');
    const messageDiv = document.createElement('div');
    messageDiv.className = 'message user-message';
    messageDiv.innerHTML = `
        <div class="message-content">
            <p>${message}</p>
        </div>
    `;
    chatMessages.appendChild(messageDiv);
    chatMessages.scrollTop = chatMessages.scrollHeight;
}

// Enhanced Search Handler
function handleSearch() {
    const chatInput = document.getElementById('chatInput');
    const query = chatInput.value.trim();
    
    // Remove welcome message if it exists
    const welcomeMessage = document.getElementById('welcomeMessage');
    if (welcomeMessage) {
        welcomeMessage.remove();
    }
    
    if (query) {
        displayUserMessage(query);
        const searchResults = processSearch(query);
        
        // Handle Q&A Results
        if (searchResults.qaAnswers.length > 0) {
            searchResults.qaAnswers.forEach(qa => {
                displayBotMessage(`<strong>${qa.category} Information:</strong> ${qa.answer}`);
            });
            return;
        }
        
        // Handle Service Results
        if (searchResults.services.size > 0) {
            const serviceArray = Array.from(searchResults.services);
            displayBotMessage(`I found these services matching your request: ${serviceArray.join(', ')}`);
            serviceArray.forEach(service => {
                const category = categories.find(cat => 
                    cat.services.some(s => s.toLowerCase() === service.toLowerCase())
                );
                if (category) {
                    showServices(category.id, service);
                }
            });
        } else {
            // Fallback for no matches
            displayBotMessage("I couldn't understand your request. Here are our popular services:");
            document.getElementById('categoryCarousel').style.display = 'flex';
        }
        
        chatInput.value = '';
    }
}

// Existing Search Initialization
function initializeSearchFunctionality() {
    const chatInput = document.getElementById('chatInput');
    const sendButton = document.getElementById('sendButton');

    sendButton.addEventListener('click', handleSearch);
    chatInput.addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
            handleSearch();
        }
    });
}

// Expose function to add more Q&A entries dynamically
function addQAEntry(question, answer, category = 'General') {
    searchConfig.qaDatabase[question.toLowerCase()] = { answer, category };
}

// Category Carousel Implementation
function initializeCategoryCarousel() {
    const categoryCarousel = document.getElementById('categoryCarousel');
    categoryCarousel.innerHTML = '';
    categories.forEach(category => {
        const categoryCard = document.createElement('div');
        categoryCard.className = 'category-card';
        categoryCard.innerHTML = `
            <div class="category-icon">
                <i class="fas ${category.icon}"></i>
            </div>
            <div class="category-name">${category.name}</div>
        `;
        categoryCard.addEventListener('click', () => showCategoryPage(category));
        categoryCarousel.appendChild(categoryCard);
    });
}

const serviceIcons = {
    'Plumbing': 'fa-faucet',
    'Electrical': 'fa-bolt',
    'Carpentry': 'fa-hammer',
    'Painting': 'fa-paint-roller',
    'Appliance Repair': 'fa-tv',
    'AC/Heating Repair': 'fa-wind',
    'Cleaning Services': 'fa-broom',
    'Skills Training': 'fa-chalkboard-teacher'
};

function showCategoryPage(category) {
    const pageGrid = document.getElementById('pageGrid');
    pageGrid.style.display = 'grid';
    pageGrid.innerHTML = '';

    category.services.forEach(service => {
        const serviceCard = document.createElement('div');
        serviceCard.className = 'category-card';
        const iconClass = serviceIcons[service] || 'fa-tools';
        
        serviceCard.innerHTML = `
            <div class="category-icon">
                <i class="fas ${iconClass}"></i>
            </div>
            <div class="category-name">${service}</div>
        `;
        serviceCard.addEventListener('click', () => showServices(category.id, service));
        pageGrid.appendChild(serviceCard);
    });

    document.getElementById('categoryCarousel').style.display = 'none';
    document.getElementById('packageGrid').style.display = 'none';
}

function showServices(categoryId, serviceName) {
    const packages = servicePackages[serviceName] || servicePackages['default'];
    const packageGrid = document.getElementById('packageGrid');
    
    packageGrid.innerHTML = '';
    
    const serviceTitle = document.createElement('div');
    serviceTitle.className = 'package-header';
    serviceTitle.innerHTML = `<h2 class="package-title">${serviceName} Packages</h2>`;
    packageGrid.appendChild(serviceTitle);
    
    Object.entries(packages).forEach(([type, package]) => {
        const packageCard = document.createElement('div');
        packageCard.className = 'package-card';
        packageCard.innerHTML = `
            <div class="package-header">
                <div class="package-title">${package.title}</div>
                <div class="package-price">
                    ${package.price}
                    <button class="info-button" onclick="showPackageInfo('${serviceName}', '${type}')">
                        <i class="fas fa-info-circle"></i>
                    </button>
                </div>
            </div>
            <div class="package-features">
                ${package.features.map(feature => `<div>• ${feature}</div>`).join('')}
            </div>
            <div class="package-actions">
                <button class="action-button book-button" 
                    onclick="showBookingForm('${categoryId}', '${serviceName}', '${type}')">
                    Book Now
                </button>
                <button class="action-button call-button" onclick="initiateCall()">
                    <i class="fas fa-phone"></i> Call
                </button>
            </div>
        `;
        packageGrid.appendChild(packageCard);
    });
    
    packageGrid.style.display = 'flex';
    document.getElementById('categoryCarousel').style.display = 'none';
    document.getElementById('pageGrid').style.display = 'none';
}

// Package Information Modal
function showPackageInfo(serviceName, packageType) {
    const packages = servicePackages[serviceName] || servicePackages['default'];
    const package = packages[packageType];
    
    let modalContainer = document.getElementById('packageInfoModal');
    if (!modalContainer) {
        modalContainer = document.createElement('div');
        modalContainer.id = 'packageInfoModal';
        document.body.appendChild(modalContainer);
    }
    
    modalContainer.innerHTML = `
        <div class="modal-overlay" onclick="closePackageInfo()">
            <div class="modal-content" onclick="event.stopPropagation()">
                <div class="modal-header">
                    <h2>${package.title}</h2>
                    <button class="close-button" onclick="closePackageInfo()">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
                <div class="modal-body">
                    <div class="package-price-section">
                        <div class="price-tag">${package.price}</div>
                        <div class="price-duration">per service</div>
                    </div>
                    
                    <div class="package-details-section">
                        <h3>Package Features</h3>
                        <ul class="feature-list">
                            ${package.features.map(feature => 
                                `<li><i class="fas fa-check"></i> ${feature}</li>`
                            ).join('')}
                        </ul>
                    </div>
                    
                    <div class="package-benefits-section">
                        <h3>Additional Benefits</h3>
                        <div class="benefits-grid">
                            ${getBenefitsByPackageType(packageType)}
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button class="action-button book-button" 
                        onclick="showBookingForm('${serviceName}', '${packageType}'); closePackageInfo();">
                        Book Now
                    </button>
                    <button class="action-button call-button" onclick="initiateCall()">
                        <i class="fas fa-phone"></i> Call for Inquiry
                    </button>
                </div>
            </div>
        </div>
    `;
    
    modalContainer.style.display = 'block';
    document.body.style.overflow = 'hidden';
}

function closePackageInfo() {
    const modalContainer = document.getElementById('packageInfoModal');
    if (modalContainer) {
        modalContainer.style.display = 'none';
        document.body.style.overflow = 'auto';
    }
}

function getBenefitsByPackageType(packageType) {
    const benefits = {
        silver: [
            { icon: 'fa-clock', text: 'Standard Response Time' },
            { icon: 'fa-tools', text: 'Basic Tools Included' },
            { icon: 'fa-phone', text: 'Phone Support' }
        ],
        gold: [
            { icon: 'fa-clock', text: 'Priority Response' },
            { icon: 'fa-tools', text: 'Professional Tools' },
            { icon: 'fa-headset', text: '24/7 Support' },
            { icon: 'fa-percent', text: '10% Off Next Booking' }
        ],
        platinum: [
            { icon: 'fa-clock', text: 'VIP Response' },
            { icon: 'fa-tools', text: 'Premium Tools' },
            { icon: 'fa-headset', text: 'Dedicated Support' },
            { icon: 'fa-percent', text: '20% Off Next Booking' },
            { icon: 'fa-shield-alt', text: 'Extended Warranty' }
        ]
    };

// Continuing from the getBenefitsByPackageType function
return benefits[packageType].map(benefit => `
        <div class="benefit-item">
            <i class="fas ${benefit.icon}"></i>
            <span>${benefit.text}</span>
        </div>
    `).join('');
}


        // Booking Form Display
        function showBookingForm(categoryId, serviceName, packageType) {
            const bookingForm = document.getElementById('bookingForm');
            bookingForm.style.display = 'block';
            bookingForm.dataset.categoryId = categoryId;
            bookingForm.dataset.serviceName = serviceName;
            bookingForm.dataset.packageType = packageType;
        }
        
        // Date and Time Picker Implementation
        function initializeDateTimePickers() {
            flatpickr("#dateInput", {
                minDate: "today",
                maxDate: new Date().fp_incr(30),
                dateFormat: "Y-m-d"
            });
        
            flatpickr("#timeInput", {
                enableTime: true,
                noCalendar: true,
                dateFormat: "H:i",
                minTime: "09:00",
                maxTime: "18:00",
                minuteIncrement: 30
            });
        }
         
                // Menu toggle functionality
                const menuButton = document.getElementById('menuButton');
        const menuOverlay = document.getElementById('menuOverlay');
        const menuItems = document.querySelectorAll('.menu-item');

        menuButton.addEventListener('click', () => {
            menuOverlay.style.display = menuOverlay.style.display === 'none' ? 'block' : 'none';
        });

        // Close menu when clicking outside
        menuOverlay.addEventListener('click', (e) => {
            if (e.target === menuOverlay) {
                menuOverlay.style.display = 'none';
            }
        });

        // Service category click handling
        menuItems.forEach(item => {
            item.addEventListener('click', () => {
                const category = item.getAttribute('data-category');
                // Handle category selection (you can add your own logic here)
                console.log(`Selected category: ${category}`);
                menuOverlay.style.display = 'none';
                
                // Example: Update chat with selected service
                const chatMessages = document.getElementById('chatMessages');
                const message = document.createElement('div');
                message.className = 'message bot-message';
                message.innerHTML = `
                    <div class="message-avatar">
                        <i class="fas fa-robot"></i>
                    </div>
                    <div class="message-content">
                        You've selected ${category} service. Would you like to book an appointment?
                    </div>
                `;
                chatMessages.appendChild(message);
                chatMessages.scrollTop = chatMessages.scrollHeight;
            });
        });

        // Call Functionality
        function initiateCall() {
            window.location.href = 'tel:9669181789';
        }
        
// Event Listeners Setup
function setupEventListeners() {
    window.addEventListener('click', function(event) {
        if (event.target.classList.contains('modal-overlay')) {
            closePackageInfo();
            closeBookingForm();
            closeConfirmation();
        }
    });
}    
    </script>
