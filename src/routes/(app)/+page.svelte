<script>
    import { onMount } from 'svelte';
  
    let isSticky = false;
  
    onMount(() => {
      const handleScroll = () => {
        isSticky = window.scrollY > 50;
      };
  
      window.addEventListener('scroll', handleScroll);
  
      return () => {
        window.removeEventListener('scroll', handleScroll);
      };
    });
  </script>
  
  <svelte:head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link
      href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap"
      rel="stylesheet"
    />
  </svelte:head>
  
 

  <style>
    :root {
      --primary: #0066cc;
      --primary-light: #3385ff;
      --primary-dark: #004999;
      --accent: #10b981;
      --accent-hover: #059669;
      --dark: #1f2937;
      --gray-100: #f4f6f8;
      --gray-200: #e5e7eb;
      --gray-300: #d1d5db;
      --gray-500: #6b7280;
      --gray-700: #374151;
      --white: #ffffff;
      --max-width: 1200px;
      --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
      --shadow: 0 4px 6px rgba(0, 0, 0, 0.05), 0 10px 15px rgba(0, 0, 0, 0.03);
      --shadow-lg: 0 10px 25px rgba(0, 0, 0, 0.07), 0 15px 30px rgba(0, 0, 0, 0.04);
      --radius: 10px;
    }
    
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }
    
    body {
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      margin: 0;
      padding: 0;
      background-color: var(--gray-100);
      color: var(--dark);
      line-height: 1.6;
    }
    
    .container {
      max-width: var(--max-width);
      margin: 0 auto;
      padding: 40px 24px;
    }
    
    .sticky-header {
      position: sticky;
      top: 0;
      z-index: 100;
      backdrop-filter: blur(10px);
      background-color: rgba(255, 255, 255, 0.9);
      box-shadow: var(--shadow);
    }
    
    .nav-container {
      max-width: var(--max-width);
      margin: 0 auto;
      padding: 0 24px;
    }
    
    .nav {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 20px 0;
    }
    
    .logo {
      display: flex;
      align-items: center;
      gap: 12px;
      text-decoration: none;
    }
    
    .logo-icon {
      background: linear-gradient(135deg, var(--primary), var(--primary-light));
      width: 40px;
      height: 40px;
      border-radius: 8px;
      display: grid;
      place-items: center;
      font-weight: 700;
      color: var(--white);
      font-size: 20px;
    }
    
    .logo-text {
      font-size: 18px;
      font-weight: 700;
      color: var(--dark);
    }
    
    .nav-links {
      display: flex;
      gap: 24px;
    }
    
    .nav-links a {
      color: var(--gray-700);
      text-decoration: none;
      font-weight: 500;
      font-size: 16px;
      padding: 8px 16px;
      border-radius: 6px;
      transition: all 0.3s ease;
    }
    
    .nav-links a:hover {
      color: var(--primary);
      background-color: rgba(0, 102, 204, 0.05);
    }
    
    .button {
      display: inline-block;
      background: var(--accent);
      color: var(--white);
      padding: 12px 24px;
      border-radius: var(--radius);
      text-decoration: none;
      font-weight: 600;
      font-size: 16px;
      transition: all 0.3s ease;
      border: none;
      cursor: pointer;
      text-align: center;
    }
    
    .button:hover {
      background: var(--accent-hover);
      transform: translateY(-2px);
      box-shadow: var(--shadow);
    }
    
    .button.outline {
      background: transparent;
      border: 2px solid var(--accent);
      color: var(--accent);
    }
    
    .button.outline:hover {
      background: var(--accent);
      color: var(--white);
    }
    
    .button.primary {
      background: var(--primary);
    }
    
    .button.primary:hover {
      background: var(--primary-dark);
    }
    
    .hero {
      padding: 80px 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      text-align: center;
      position: relative;
      overflow: hidden;
    }
    
    .hero-background {
      position: absolute;
      width: 100%;
      height: 100%;
      top: 0;
      left: 0;
      background: linear-gradient(135deg, rgba(0, 102, 204, 0.03), rgba(0, 153, 255, 0.06));
      z-index: -1;
    }
    
    .hero-content {
      max-width: 800px;
      margin: 0 auto;
    }
    
    .hero h1 {
      font-size: 48px;
      font-weight: 800;
      margin-bottom: 24px;
      color: var(--dark);
      line-height: 1.2;
    }
    
    .hero p {
      font-size: 20px;
      color: var(--gray-700);
      max-width: 600px;
      margin: 0 auto 40px;
      font-weight: 400;
    }
    
    .hero-buttons {
      display: flex;
      gap: 16px;
      justify-content: center;
      flex-wrap: wrap;
    }
    
    .section-title {
      text-align: center;
      margin-bottom: 60px;
    }
    
    .section-title h2 {
      font-size: 32px;
      font-weight: 700;
      color: var(--dark);
      margin-bottom: 16px;
    }
    
    .section-title p {
      font-size: 18px;
      color: var(--gray-500);
      max-width: 600px;
      margin: 0 auto;
    }
    
    .features {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      gap: 32px;
      margin-top: 60px;
    }
    
    .feature {
      background-color: var(--white);
      padding: 32px;
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      transition: all 0.3s ease;
      border: 1px solid var(--gray-200);
      height: 100%;
      display: flex;
      flex-direction: column;
    }
    
    .feature:hover {
      transform: translateY(-6px);
      box-shadow: var(--shadow-lg);
    }
    
    .feature-icon {
      width: 60px;
      height: 60px;
      background: linear-gradient(135deg, var(--primary-light), var(--primary));
      border-radius: 12px;
      display: grid;
      place-items: center;
      margin-bottom: 24px;
      color: var(--white);
      font-size: 24px;
    }
    
    .feature h3 {
      font-size: 20px;
      color: var(--primary);
      margin-bottom: 16px;
      font-weight: 600;
    }
    
    .feature p {
      color: var(--gray-700);
      font-size: 16px;
      line-height: 1.6;
      margin-bottom: 24px;
      flex-grow: 1;
    }
    
    .feature-link {
      color: var(--primary);
      text-decoration: none;
      font-weight: 600;
      display: flex;
      align-items: center;
      gap: 8px;
      transition: gap 0.3s ease;
    }
    
    .feature-link:hover {
      gap: 12px;
    }
    
    .cta {
      background: linear-gradient(135deg, var(--primary-dark), var(--primary));
      padding: 80px 32px;
      border-radius: var(--radius);
      margin-top: 100px;
      color: var(--white);
      text-align: center;
    }
    
    .cta h2 {
      font-size: 32px;
      font-weight: 700;
      margin-bottom: 20px;
    }
    
    .cta p {
      font-size: 18px;
      max-width: 600px;
      margin: 0 auto 32px;
      opacity: 0.9;
    }
    
    .footer {
      background-color: var(--dark);
      color: var(--gray-300);
      padding: 60px 0 40px;
      margin-top: 100px;
    }
    
    .footer-content {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 40px;
      max-width: var(--max-width);
      margin: 0 auto;
      padding: 0 24px;
    }
    
    .footer-col h4 {
      color: var(--white);
      font-size: 18px;
      font-weight: 600;
      margin-bottom: 24px;
    }
    
    .footer-links {
      list-style: none;
    }
    
    .footer-links li {
      margin-bottom: 12px;
    }
    
    .footer-links a {
      color: var(--gray-300);
      text-decoration: none;
      transition: color 0.3s;
    }
    
    .footer-links a:hover {
      color: var(--white);
    }
    
    .copyright {
      text-align: center;
      padding-top: 40px;
      max-width: var(--max-width);
      margin: 0 auto;
      border-top: 1px solid rgba(255, 255, 255, 0.1);
      margin-top: 40px;
      color: var(--gray-500);
      font-size: 14px;
      padding-left: 24px;
      padding-right: 24px;
    }
    
    @media (max-width: 768px) {
      .hero h1 {
        font-size: 36px;
      }
      
      .hero p {
        font-size: 18px;
      }
      
      .features {
        grid-template-columns: 1fr;
      }
      
      .hero-buttons {
        flex-direction: column;
        width: 100%;
      }
      
      .hero-buttons .button {
        width: 100%;
      }
      
      .nav-links {
        display: none;
      }
    }
  </style>
  <!-- Sticky Navigation -->
  <div class="sticky-header" class:isSticky>
    <div class="nav-container">
      <nav class="nav">
        <a href="#" class="logo">
          <div class="logo-icon">PA</div>
          <div class="logo-text">Professional Accounting</div>
        </a>
        <div class="nav-links">
          <a href="#home">Home</a>
          <a href="#services">Services</a>
          <a href="#about">About</a>
          <a href="#contact">Contact</a>
        </div>
      </nav>
    </div>
  </div>
  
  <div class="container">
    <!-- Hero Section -->
    <section id="home" class="hero">
      <div class="hero-background" />
      <div class="hero-content">
        <h1>Elevate Your Financial Management</h1>
        <p>
          Our advanced accounting solutions help businesses optimize financial
          processes, simplify taxation, and gain valuable insights for strategic
          decision-making.
        </p>
        <div class="hero-buttons">
          <a href="/dash" class="button primary">Schedule a Consultation</a>
          <a href="/dash" class="button outline">Explore Services</a>
        </div>
      </div>
    </section>
  
    <!-- Services Section -->
    <section id="services">
      <div class="section-title">
        <h2>Comprehensive Financial Solutions</h2>
        <p>
          Tailored accounting services designed to meet the unique needs of your
          business
        </p>
      </div>
  
      <div class="features">
        <div class="feature">
          <div class="feature-icon">T</div>
          <h3>Advanced Tax Planning</h3>
          <p>
            Strategic tax planning and preparation services to minimize liabilities
            and ensure compliance with the latest regulations across multiple
            jurisdictions.
          </p>
          <a href="#" class="feature-link">Learn more →</a>
        </div>
  
        <div class="feature">
          <div class="feature-icon">I</div>
          <h3>Intelligent Invoicing</h3>
          <p>
            Streamline your billing process with automated invoicing, payment
            tracking, and customizable templates that reflect your brand identity.
          </p>
          <a href="#" class="feature-link">Learn more →</a>
        </div>
  
        <div class="feature">
          <div class="feature-icon">R</div>
          <h3>Real-time Financial Reporting</h3>
          <p>
            Access comprehensive financial reports with intuitive visualizations to
            monitor performance and make data-driven decisions.
          </p>
          <a href="#" class="feature-link">Learn more →</a>
        </div>
  
        <div class="feature">
          <div class="feature-icon">C</div>
          <h3>Cash Flow Management</h3>
          <p>
            Optimize your cash flow with advanced forecasting tools, receivables
            management, and strategic planning assistance.
          </p>
          <a href="#" class="feature-link">Learn more →</a>
        </div>
  
        <div class="feature">
          <div class="feature-icon">P</div>
          <h3>Payroll Solutions</h3>
          <p>
            Comprehensive payroll processing with automated tax calculations,
            direct deposits, and compliance management for businesses of all sizes.
          </p>
          <a href="#" class="feature-link">Learn more →</a>
        </div>
  
        <div class="feature">
          <div class="feature-icon">A</div>
          <h3>Audit & Assurance</h3>
          <p>
            Professional audit services to verify financial integrity, identify
            risks, and ensure compliance with industry standards and regulations.
          </p>
          <a href="#" class="feature-link">Learn more →</a>
        </div>
      </div>
    </section>
  
    <!-- Call to Action -->
    <section id="contact" class="cta">
      <h2>Ready to Transform Your Financial Operations?</h2>
      <p>
        Our team of certified accountants is ready to help you streamline your
        finances and focus on growing your business.
      </p>
      <a href="#contact" class="button">Get Started Today</a>
    </section>
  </div>
  
  <!-- Footer -->
  <footer class="footer">
    <div class="footer-content">
      <div class="footer-col">
        <h4>Professional Accounting</h4>
        <ul class="footer-links">
          <li><a href="#home">Home</a></li>
          <li><a href="#services">Services</a></li>
          <li><a href="#about">About Us</a></li>
          <li><a href="#careers">Careers</a></li>
        </ul>
      </div>
  
      <div class="footer-col">
        <h4>Services</h4>
        <ul class="footer-links">
          <li><a href="#">Tax Planning</a></li>
          <li><a href="#">Financial Reporting</a></li>
          <li><a href="#">Payroll Management</a></li>
          <li><a href="#">Business Advisory</a></li>
        </ul>
      </div>
  
      <div class="footer-col">
        <h4>Resources</h4>
        <ul class="footer-links">
          <li><a href="#">Blog</a></li>
          <li><a href="#">Tax Resources</a></li>
          <li><a href="#">Financial Calculators</a></li>
          <li><a href="#">FAQ</a></li>
        </ul>
      </div>
  
      <div class="footer-col">
        <h4>Contact Us</h4>
        <ul class="footer-links">
          <li>123 Finance Street</li>
          <li>New York, NY 10001</li>
          <li>contact@professionalaccounting.com</li>
          <li>(555) 123-4567</li>
        </ul>
      </div>
    </div>
  
    <div class="copyright">
      <p>© 2025 Professional Accounting Solutions. All rights reserved.</p>
    </div>
  </footer>
  