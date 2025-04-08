<!-- App.svelte -->
<script>
  import { onMount } from 'svelte';
  
  let isMenuOpen = false;
  let activeSection = 'accueil';
  
  function toggleMenu() {
    isMenuOpen = !isMenuOpen;
  }
  
  function scrollToSection(id) {
    const element = document.getElementById(id);
    if (element) {
      isMenuOpen = false;
      activeSection = id;
      element.scrollIntoView({ behavior: 'smooth' });
    }
  }
  
  onMount(() => {
    const sections = document.querySelectorAll('section');
    
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          activeSection = entry.target.id;
        }
      });
    }, { threshold: 0.5 });
    
    sections.forEach(section => {
      observer.observe(section);
    });
    
    return () => {
      sections.forEach(section => {
        observer.unobserve(section);
      });
    };
  });
</script>

<header>
  <div class="container">
    <nav>
      <div class="logo">
        <img src="/api/placeholder/40/40" alt="TimePro Logo" />
        <span>TimePro</span>
      </div>
      
      <div class="nav-toggle" on:click={toggleMenu}>
        <span></span>
        <span></span>
        <span></span>
      </div>
      
      <ul class="nav-links" class:active={isMenuOpen}>
        <li><a href="#accueil" class:active={activeSection === 'accueil'} on:click|preventDefault={() => scrollToSection('accueil')}>Accueil</a></li>
        <li><a href="#fonctionnalites" class:active={activeSection === 'fonctionnalites'} on:click|preventDefault={() => scrollToSection('fonctionnalites')}>Fonctionnalités</a></li>
        <li><a href="#avantages" class:active={activeSection === 'avantages'} on:click|preventDefault={() => scrollToSection('avantages')}>Avantages</a></li>
        <li><a href="#tarifs" class:active={activeSection === 'tarifs'} on:click|preventDefault={() => scrollToSection('tarifs')}>Tarifs</a></li>
        <li><a href="#temoignages" class:active={activeSection === 'temoignages'} on:click|preventDefault={() => scrollToSection('temoignages')}>Témoignages</a></li>
        <li><a href="#contact" class:active={activeSection === 'contact'} on:click|preventDefault={() => scrollToSection('contact')}>Contact</a></li>
      </ul>
      
      <div class="action-buttons">
        <button class="btn btn-secondary">Se connecter</button>
        <button class="btn btn-primary">Essai gratuit</button>
      </div>
    </nav>
  </div>
</header>

<main>
  <section id="accueil" class="hero">
    <div class="container">
      <div class="hero-content">
        <h1>Simplifiez la gestion du temps de vos employés</h1>
        <p>TimePro est le logiciel de gestion de temps le plus complet pour les entreprises modernes. Suivi des heures, planification, rapports et bien plus.</p>
        <div class="hero-buttons">
          <button class="btn btn-primary btn-lg">Commencer maintenant</button>
          <button class="btn btn-outline btn-lg">Voir la démo</button>
        </div>
      </div>
      <div class="hero-image">
        <img src="/api/placeholder/500/400" alt="Démonstration TimePro" />
      </div>
    </div>
  </section>

  <section id="fonctionnalites" class="features">
    <div class="container">
      <div class="section-header">
        <h2>Fonctionnalités puissantes</h2>
        <p>Découvrez comment TimePro peut transformer votre gestion des ressources humaines</p>
      </div>
      
      <div class="features-grid">
        <div class="feature-card">
          <div class="feature-icon">📊</div>
          <h3>Suivi du temps en temps réel</h3>
          <p>Suivez les heures de travail de vos employés avec précision grâce à notre système intuitif.</p>
        </div>
        
        <div class="feature-card">
          <div class="feature-icon">📆</div>
          <h3>Planification intelligente</h3>
          <p>Créez des plannings optimisés en fonction des compétences et de la disponibilité.</p>
        </div>
        
        <div class="feature-card">
          <div class="feature-icon">📱</div>
          <h3>Application mobile</h3>
          <p>Accédez à toutes les fonctionnalités depuis n'importe où grâce à notre application mobile.</p>
        </div>
        
        <div class="feature-card">
          <div class="feature-icon">📈</div>
          <h3>Rapports détaillés</h3>
          <p>Générez des rapports personnalisés pour analyser la productivité et optimiser les ressources.</p>
        </div>
        
        <div class="feature-card">
          <div class="feature-icon">🔔</div>
          <h3>Alertes et notifications</h3>
          <p>Recevez des alertes pour les retards, absences ou dépassements d'heures.</p>
        </div>
        
        <div class="feature-card">
          <div class="feature-icon">🔄</div>
          <h3>Intégrations</h3>
          <p>Connectez TimePro à vos outils existants comme la paie, RH ou comptabilité.</p>
        </div>
      </div>
    </div>
  </section>

  <section id="avantages" class="benefits">
    <div class="container">
      <div class="section-header">
        <h2>Pourquoi choisir TimePro?</h2>
        <p>Nos clients constatent une amélioration de 30% de leur productivité</p>
      </div>
      
      <div class="benefits-content">
        <div class="benefits-image">
          <img src="/api/placeholder/450/350" alt="Avantages TimePro" />
        </div>
        
        <div class="benefits-list">
          <div class="benefit-item">
            <h3>Économisez du temps</h3>
            <p>Automatisez les tâches administratives chronophages et concentrez-vous sur ce qui compte vraiment.</p>
          </div>
          
          <div class="benefit-item">
            <h3>Réduisez les erreurs</h3>
            <p>Éliminez les erreurs manuelles dans le suivi du temps et le calcul des heures travaillées.</p>
          </div>
          
          <div class="benefit-item">
            <h3>Augmentez la satisfaction</h3>
            <p>Offrez à vos employés un système transparent et équitable de gestion du temps.</p>
          </div>
          
          <div class="benefit-item">
            <h3>Optimisez les coûts</h3>
            <p>Réduisez les heures supplémentaires non nécessaires et optimisez l'allocation des ressources.</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <section id="tarifs" class="pricing">
    <div class="container">
      <div class="section-header">
        <h2>Nos formules d'abonnement</h2>
        <p>Des solutions adaptées à toutes les tailles d'entreprise</p>
      </div>
      
      <div class="pricing-plans">
        <div class="pricing-card">
          <div class="pricing-header">
            <h3>Démarrage</h3>
            <div class="pricing-price">
              <span class="currency">€</span>
              <span class="amount">9</span>
              <span class="period">/utilisateur/mois</span>
            </div>
          </div>
          
          <ul class="pricing-features">
            <li>Jusqu'à 10 utilisateurs</li>
            <li>Suivi du temps de base</li>
            <li>Rapports standards</li>
            <li>Application mobile</li>
          </ul>
          
          <button class="btn btn-primary">Essai gratuit</button>
        </div>
        
        <div class="pricing-card featured">
          <div class="pricing-header">
            <h3>Professionnel</h3>
            <div class="pricing-price">
              <span class="currency">€</span>
              <span class="amount">19</span>
              <span class="period">/utilisateur/mois</span>
            </div>
          </div>
          
          <ul class="pricing-features">
            <li>Utilisateurs illimités</li>
            <li>Suivi du temps avancé</li>
            <li>Rapports personnalisés</li>
            <li>Application mobile</li>
            <li>Planning automatisé</li>
            <li>Intégrations</li>
          </ul>
          
          <button class="btn btn-accent">Commencer maintenant</button>
        </div>
        
        <div class="pricing-card">
          <div class="pricing-header">
            <h3>Entreprise</h3>
            <div class="pricing-price">
              <span class="currency">€</span>
              <span class="amount">29</span>
              <span class="period">/utilisateur/mois</span>
            </div>
          </div>
          
          <ul class="pricing-features">
            <li>Utilisateurs illimités</li>
            <li>Toutes les fonctionnalités</li>
            <li>API complète</li>
            <li>Support dédié 24/7</li>
            <li>Formation personnalisée</li>
            <li>Hébergement sécurisé</li>
          </ul>
          
          <button class="btn btn-primary">Contacter l'équipe</button>
        </div>
      </div>
    </div>
  </section>

  <section id="temoignages" class="testimonials">
    <div class="container">
      <div class="section-header">
        <h2>Ce que nos clients disent</h2>
        <p>Des milliers d'entreprises font confiance à TimePro</p>
      </div>
      
      <div class="testimonials-slider">
        <div class="testimonial-card">
          <div class="testimonial-content">
            <p>"TimePro a transformé notre façon de gérer les horaires. Nous avons constaté une réduction de 25% des heures supplémentaires dès le premier mois."</p>
          </div>
          <div class="testimonial-author">
            <img src="/api/placeholder/60/60" alt="Photo de profil" />
            <div>
              <h4>Marie Dupont</h4>
              <p>DRH, Entreprise ABC</p>
            </div>
          </div>
        </div>
        
        <div class="testimonial-card">
          <div class="testimonial-content">
            <p>"L'interface intuitive et les rapports détaillés nous ont permis d'identifier rapidement les inefficacités dans notre allocation des ressources."</p>
          </div>
          <div class="testimonial-author">
            <img src="/api/placeholder/60/60" alt="Photo de profil" />
            <div>
              <h4>Thomas Martin</h4>
              <p>Directeur des opérations, XYZ Tech</p>
            </div>
          </div>
        </div>
        
        <div class="testimonial-card">
          <div class="testimonial-content">
            <p>"Nos employés apprécient la transparence et la simplicité du système. La satisfaction au travail a augmenté significativement depuis l'implémentation."</p>
          </div>
          <div class="testimonial-author">
            <img src="/api/placeholder/60/60" alt="Photo de profil" />
            <div>
              <h4>Sophie Legrand</h4>
              <p>CEO, Startup Innovante</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <section id="contact" class="contact">
    <div class="container">
      <div class="section-header">
        <h2>Contactez-nous</h2>
        <p>Nous sommes là pour répondre à toutes vos questions</p>
      </div>
      
      <div class="contact-content">
        <div class="contact-form">
          <form>
            <div class="form-group">
              <label for="name">Nom complet</label>
              <input type="text" id="name" placeholder="Votre nom" required />
            </div>
            
            <div class="form-group">
              <label for="email">Email professionnel</label>
              <input type="email" id="email" placeholder="votre@email.com" required />
            </div>
            
            <div class="form-group">
              <label for="company">Entreprise</label>
              <input type="text" id="company" placeholder="Nom de votre entreprise" />
            </div>
            
            <div class="form-group">
              <label for="message">Message</label>
              <textarea id="message" rows="5" placeholder="Comment pouvons-nous vous aider?" required></textarea>
            </div>
            
            <button type="submit" class="btn btn-primary btn-block">Envoyer le message</button>
          </form>
        </div>
        
        <div class="contact-info">
          <div class="info-item">
            <div class="info-icon">📍</div>
            <div>
              <h4>Adresse</h4>
              <p>123 Avenue des Affaires, 75000 Paris, France</p>
            </div>
          </div>
          
          <div class="info-item">
            <div class="info-icon">📞</div>
            <div>
              <h4>Téléphone</h4>
              <p>+33 1 23 45 67 89</p>
            </div>
          </div>
          
          <div class="info-item">
            <div class="info-icon">✉️</div>
            <div>
              <h4>Email</h4>
              <p>contact@timepro.com</p>
            </div>
          </div>
          
          <div class="social-links">
            <a href="#" class="social-link">Facebook</a>
            <a href="#" class="social-link">Twitter</a>
            <a href="#" class="social-link">LinkedIn</a>
            <a href="#" class="social-link">Instagram</a>
          </div>
        </div>
      </div>
    </div>
  </section>
</main>

<footer>
  <div class="container">
    <div class="footer-content">
      <div class="footer-logo">
        <img src="/api/placeholder/40/40" alt="TimePro Logo" />
        <span>TimePro</span>
      </div>
      
      <div class="footer-links">
        <div class="footer-column">
          <h4>Produit</h4>
          <ul>
            <li><a href="#fonctionnalites">Fonctionnalités</a></li>
            <li><a href="#tarifs">Tarifs</a></li>
            <li><a href="#">Démo</a></li>
            <li><a href="#">Mises à jour</a></li>
          </ul>
        </div>
        
        <div class="footer-column">
          <h4>Entreprise</h4>
          <ul>
            <li><a href="#">À propos</a></li>
            <li><a href="#temoignages">Clients</a></li>
            <li><a href="#">Carrières</a></li>
            <li><a href="#">Blog</a></li>
          </ul>
        </div>
        
        <div class="footer-column">
          <h4>Support</h4>
          <ul>
            <li><a href="#contact">Contact</a></li>
            <li><a href="#">Documentation</a></li>
            <li><a href="#">Tutoriels</a></li>
            <li><a href="#">FAQ</a></li>
          </ul>
        </div>
        
        <div class="footer-column">
          <h4>Légal</h4>
          <ul>
            <li><a href="#">Conditions</a></li>
            <li><a href="#">Confidentialité</a></li>
            <li><a href="#">Cookies</a></li>
            <li><a href="#">Sécurité</a></li>
          </ul>
        </div>
      </div>
    </div>
    
    <div class="footer-bottom">
      <p>&copy; 2025 TimePro. Tous droits réservés.</p>
      <div class="language-selector">
        <select>
          <option value="fr">Français</option>
          <option value="en">English</option>
          <option value="es">Español</option>
          <option value="de">Deutsch</option>
        </select>
      </div>
    </div>
  </div>
</footer>

<style>
  /* Styles globaux */
  :global(body) {
    margin: 0;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    line-height: 1.6;
    color: #333;
    background-color: #f8f9fa;
  }
  
  .container {
    width: 100%;
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
  }
  
  /* Header et navigation */
  header {
    background-color: #2c3e50;
    color: #fff;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1000;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  
  nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px 0;
  }
  
  .logo {
    display: flex;
    align-items: center;
    font-size: 1.5rem;
    font-weight: bold;
    color: #fff;
  }
  
  .logo img {
    margin-right: 10px;
  }
  
  .nav-links {
    display: flex;
    list-style: none;
    margin: 0;
    padding: 0;
  }
  
  .nav-links li {
    margin: 0 15px;
  }
  
  .nav-links a {
    color: #fff;
    text-decoration: none;
    font-weight: 500;
    transition: color 0.3s;
    padding: 5px 0;
    position: relative;
  }
  
  .nav-links a:after {
    content: '';
    position: absolute;
    width: 0;
    height: 2px;
    bottom: 0;
    left: 0;
    background-color: #3498db;
    transition: width 0.3s;
  }
  
  .nav-links a:hover:after, .nav-links a.active:after {
    width: 100%;
  }
  
  .action-buttons {
    display: flex;
  }
  
  .nav-toggle {
    display: none;
    flex-direction: column;
    cursor: pointer;
  }
  
  .nav-toggle span {
    width: 25px;
    height: 3px;
    background-color: #fff;
    margin: 2px 0;
    transition: all 0.3s;
  }
  
  /* Boutons */
  .btn {
    display: inline-block;
    padding: 10px 20px;
    border-radius: 4px;
    font-weight: 500;
    text-align: center;
    cursor: pointer;
    transition: all 0.3s;
    border: none;
    outline: none;
  }
  
  .btn-primary {
    background-color: #3498db;
    color: white;
  }
  
  .btn-primary:hover {
    background-color: #2980b9;
  }
  
  .btn-secondary {
    background-color: transparent;
    color: #fff;
    border: 1px solid #fff;
  }
  
  .btn-secondary:hover {
    background-color: rgba(255, 255, 255, 0.1);
  }
  
  .btn-accent {
    background-color: #e74c3c;
    color: white;
  }
  
  .btn-accent:hover {
    background-color: #c0392b;
  }
  
  .btn-outline {
    background-color: transparent;
    color: #3498db;
    border: 1px solid #3498db;
  }
  
  .btn-outline:hover {
    background-color: #3498db;
    color: white;
  }
  
  .btn-lg {
    padding: 12px 30px;
    font-size: 1.1rem;
  }
  
  .btn-block {
    display: block;
    width: 100%;
  }
  
  /* Sections */
  section {
    padding: 100px 0;
  }
  
  .section-header {
    text-align: center;
    margin-bottom: 60px;
  }
  
  .section-header h2 {
    font-size: 2.5rem;
    margin-bottom: 15px;
    color: #2c3e50;
  }
  
  .section-header p {
    font-size: 1.1rem;
    color: #7f8c8d;
    max-width: 700px;
    margin: 0 auto;
  }
  
  /* Hero Section */
  .hero {
    background: linear-gradient(135deg, #3498db, #2c3e50);
    color: white;
    padding: 180px 0 100px;
  }
  
  .hero-content {
    max-width: 600px;
  }
  
  .hero h1 {
    font-size: 3rem;
    font-weight: 700;
    margin-bottom: 20px;
    line-height: 1.2;
  }
  
  .hero p {
    font-size: 1.2rem;
    margin-bottom: 30px;
    opacity: 0.9;
  }
  
  .hero-buttons {
    display: flex;
    gap: 15px;
  }
  
  .hero-image {
    position: absolute;
    right: 0;
    top: 50%;
    transform: translateY(-50%);
    max-width: 50%;
    display: none;
  }
  
  /* Features Section */
  .features {
    background-color: #fff;
  }
  
  .features-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 30px;
  }
  
  .feature-card {
    background-color: #f8f9fa;
    border-radius: 10px;
    padding: 30px;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
    transition: transform 0.3s, box-shadow 0.3s;
  }
  
  .feature-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  }
  
  .feature-icon {
    font-size: 2.5rem;
    margin-bottom: 20px;
  }
  
  .feature-card h3 {
    font-size: 1.5rem;
    margin-bottom: 15px;
    color: #2c3e50;
  }
  
  /* Benefits Section */
  .benefits {
    background-color: #f8f9fa;
  }
  
  .benefits-content {
    display: flex;
    align-items: center;
    gap: 60px;
  }
  
  .benefits-image {
    flex: 1;
  }
  
  .benefits-list {
    flex: 1;
  }
  
  .benefit-item {
    margin-bottom: 30px;
  }
  
  .benefit-item h3 {
    font-size: 1.4rem;
    margin-bottom: 10px;
    color: #2c3e50;
  }
  
  /* Pricing Section */
  .pricing {
    background-color: #fff;
  }
  
  .pricing-plans {
    display: flex;
    justify-content: center;
    gap: 30px;
    flex-wrap: wrap;
  }
  
  .pricing-card {
    background-color: #f8f9fa;
    border-radius: 10px;
    padding: 40px 30px;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
    flex: 1;
    min-width: 280px;
    max-width: 350px;
    transition: transform 0.3s;
  }
  
  .pricing-card.featured {
    background-color: #2c3e50;
    color: white;
    transform: scale(1.05);
    z-index: 1;
  }
  
  .pricing-card:hover {
    transform: translateY(-10px);
  }
  
  .pricing-card.featured:hover {
    transform: translateY(-10px) scale(1.05);
  }
  
  .pricing-header h3 {
    font-size: 1.8rem;
    margin-bottom: 15px;
    color: inherit;
  }
  
  .pricing-price {
    font-size: 2.5rem;
    font-weight: 700;
    margin-bottom: 30px;
  }
  
  .pricing-price .currency {
    font-size: 1.5rem;
    vertical-align: top;
  }
  
  .pricing-price .period {
    font-size: 1rem;
    opacity: 0.7;
  }
  
  .pricing-features {
    list-style: none;
    padding: 0;
    margin: 0 0 30px;
  }
  
  .pricing-features li {
    padding: 10px 0;
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  }
  
  .pricing-card.featured .pricing-features li {
    border-color: rgba(255, 255, 255, 0.1);
  }
  
  /* Testimonials Section */
  .testimonials {
    background-color: #f8f9fa;
  }
  
  .testimonials-slider {
    display: flex;
    gap: 30px;
    overflow-x: auto;
    padding: 20px 0;
    scroll-snap-type: x mandatory;
    -webkit-overflow-scrolling: touch;
  }
  
  .testimonial-card {
    background-color: #fff;
    border-radius: 10px;
    padding: 30px;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
    flex: 0 0 350px;
    scroll-snap-align: start;
  }
  
  .testimonial-content p {
    font-style: italic;
    margin-bottom: 20px;
  }
  
  .testimonial-author {
    display: flex;
    align-items: center;
  }
  
  .testimonial-author img {
    border-radius: 50%;
    margin-right: 15px;
  }
  
  .testimonial-author h4 {
    margin: 0;
    font-size: 1.1rem;
  }
  
  .testimonial-author p {
    margin: 5px 0 0;
    color: #7f8c8d;
    font-size: 0.9rem;
  }
  
  /* Contact Section */
  .contact {
    background-color: #fff;
  }
  
  .contact-content {
    display: flex;
    gap: 60px;
  }
  
  .contact-form {
    flex: 1;
  }
  
  .form-group {
    margin-bottom: 20px;
  }
  
  .form-group label {
    display: block;
    margin-bottom: 8px;
    font-weight: 500;
  }
  
  .form-group input,
  .form-group textarea {
    width: 100%;
    padding: 12px 15px;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 1rem;
    transition: border-color 0.3s;
  }
  
  .form-group input:focus,
  .form-group textarea:focus {
    border-color: #3498db;
    outline: none;
  }
  
  .contact-info {
    flex: 1;
  }
  
  .info-item {
    display: flex;
    align-items: flex-start;
    margin-bottom: 25px;
  }
  
  .info-icon {
    font-size: 1.5rem;
    margin-right: 15px;
  }
  
  .info-item h4 {
    margin: 0 0 5px;
    font-size: 1.2rem;
  }
  
  .social-links {
    display: flex;
    gap: 15px;
    margin-top: 30px;
  }
  
  .social-link {
    display: inline-block;
    padding: 8px 12px;
    border-radius: 4px;
    background-color: #3498db;
    color: white;
    text-decoration: none;
    font-size: 0.9rem;
    transition: background-color 0.3s;
  }
  
  .social-link:hover {
    background-color: #2980b9;
  }
  
  /* Footer */
  footer {
    background-color: #2c3e50;
    color: #fff;
    padding: 60px 0 30px;
  }
  
  .footer-content {
    display: flex;
    flex-wrap: wrap;
    gap: 40px;
    margin-bottom: 40px;
  }
  
  .footer-logo {
    display: flex;
    align-items: center;
    font-size: 1.5rem;
    font-weight: bold;
    color: #fff;
    margin-right: 40px;
  }
  
  .footer-logo img {
    margin-right: 10px;
  }
  
  .footer-links {
    display: flex;
    flex: 1;
    flex-wrap: wrap;
    gap: 30px;
  }
  
  .footer-column {
    flex: 1;
    min-width: 150px;
  }
  
  .footer-column h4 {
    font-size: 1.2rem;
    margin-bottom: 20px;
    color: #fff;
  }
  
  .footer-column ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  
  .footer-column li {
    margin-bottom: 10px;
  }
  
  .footer-column a {
    color: rgba(255, 255, 255, 0.7);
    text-decoration: none;
    transition: color 0.3s;
  }
  
  .footer-column a:hover {
    color: #3498db;
  }
  
  .footer-bottom {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-top: 30px;
    border-top: 1px solid rgba(255, 255, 255, 0.1);
  }
  
  .footer-bottom p {
    margin: 0;
    color: rgba(255, 255, 255, 0.7);
  }
  
  .language-selector select {
    background-color: transparent;
    color: #fff;
    border: 1px solid rgba(255, 255, 255, 0.3);
    padding: 5px 10px;
    border-radius: 4px;
    outline: none;
  }
  
  .language-selector option {
    background-color: #2c3e50;
    color: #fff;
  }
  
  /* Responsive Styles */
  @media (max-width: 992px) {
    .benefits-content {
      flex-direction: column;
    }
    
    .contact-content {
      flex-direction: column;
    }
    
    .hero-image {
      display: none;
    }
  }
  
  @media (max-width: 768px) {
    .nav-toggle {
      display: flex;
    }
    
    .nav-links {
      position: absolute;
      top: 100%;
      left: 0;
      right: 0;
      flex-direction: column;
      background-color: #2c3e50;
      padding: 20px;
      box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
      transform: translateY(-150%);
      transition: transform 0.3s;
      opacity: 0;
    }
    
    .nav-links.active {
      transform: translateY(0);
      opacity: 1;
    }
    
    .nav-links li {
      margin: 10px 0;
    }
    
    .action-buttons {
      display: none;
    }
    
    .pricing-card {
      max-width: 100%;
    }
    
    .footer-content {
      flex-direction: column;
      gap: 30px;
    }
    
    .footer-links {
      flex-direction: column;
    }
    
    .footer-bottom {
      flex-direction: column;
      gap: 20px;
    }
  }
</style>