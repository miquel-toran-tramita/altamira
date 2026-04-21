<script>
  let status = $state(""); // "", "sending", "success", "error"

  async function handleSubmit(event) {
    event.preventDefault();
    status = "sending";

    const form = event.target;
    const data = new FormData(form);

    try {
      const response = await fetch(form.action, {
        method: form.method,
        body: data,
        headers: {
          Accept: "application/json",
        },
      });

      if (response.ok) {
        status = "success";
        form.reset();
      } else {
        status = "error";
      }
    } catch (error) {
      status = "error";
    }
  }
</script>

<div class="contact-container">
  <div class="contact-info">
    <h4>¿Tienes un proyecto en mente?</h4>
    <p>
      Si necesitas un presupuesto para tu próxima obra, estaremos encantados de
      ayudarte. Nuestro equipo analizará tus necesidades para ofrecerte la mejor
      solución estructural.
    </p>

    <div class="info-item">
      <span class="label">Oficina</span>
      <span>Carrer de Pere Martell, 76, 08917, Badalona</span>
    </div>

    <div class="info-item">
      <span class="label">Email</span>
      <a href="mailto:ruben@construccionesaltamira.com"
        >ruben@construccionesaltamira.com</a
      >
    </div>

    <div class="info-item">
      <span class="label">Teléfono</span>
      <a href="tel:+34634952132">634 952 132</a>
    </div>
  </div>

  <div class="contact-form">
    {#if status === "success"}
      <div class="success-message">
        <h5>¡Mensaje enviado!</h5>
        <p>
          Gracias por contactar con nosotros. Te responderemos lo antes posible.
        </p>
        <button onclick={() => (status = "")}>Enviar otro mensaje</button>
      </div>
    {:else}
      <form
        action="https://formspree.io/f/mgorlrad"
        method="POST"
        onsubmit={handleSubmit}
      >
        <div class="form-group">
          <label for="name">Nombre completo</label>
          <input
            type="text"
            id="name"
            name="name"
            placeholder="Tu nombre..."
            required
          />
        </div>
        <div class="form-group">
          <label for="email">Correo electrónico</label>
          <input
            type="email"
            id="email"
            name="email"
            placeholder="tu@email.com"
            required
          />
        </div>
        <div class="form-group">
          <label for="message">Mensaje</label>
          <textarea
            id="message"
            name="message"
            placeholder="Cuéntanos más sobre tu proyecto..."
            required
          ></textarea>
        </div>
        {#if status === "error"}
          <p class="error-text">
            Hubo un error al enviar el mensaje. Por favor, inténtalo de nuevo.
          </p>
        {/if}
        <button type="submit" disabled={status === "sending"}>
          {status === "sending" ? "Enviando..." : "Enviar mensaje"}
        </button>
      </form>
    {/if}
  </div>
</div>

<style lang="scss">
  .contact-container {
    display: grid;
    grid-template-columns: 1fr 1.2fr;
    gap: 100px;

    @media (max-width: 1100px) {
      gap: 50px;
    }

    @media (max-width: 1100px) {
      grid-template-columns: 1fr;
      gap: 60px;
    }
  }

  .contact-info {
    h4 {
      font-family: var(--fontPrimary);
      font-size: 32px;
      margin-bottom: 30px;
      color: var(--colorText);

      @media (max-width: 768px) {
        font-size: 24px;
        margin-bottom: 20px;
      }
    }

    p {
      font-family: var(--fontSecondary);
      font-size: 18px;
      color: var(--colorText2);
      line-height: 1.6;
      margin-bottom: 40px;
    }

    .info-item {
      display: flex;
      flex-direction: column;
      gap: 5px;
      margin-bottom: 30px;

      .label {
        font-family: var(--fontSecondary);
        font-size: 12px;
        text-transform: uppercase;
        letter-spacing: 2px;
        color: var(--colorPrimary);
        font-weight: 700;
        display: block; // added for span
      }

      span,
      a {
        font-family: var(--fontSecondary);
        font-size: 20px;
        color: var(--colorText);
        text-decoration: none;
        transition: color 0.3s ease;

        &:hover {
          color: var(--colorPrimary);
        }
      }
    }
  }

  .contact-form {
    background: var(--colorSecondary);
    padding: 50px;
    border: 1px solid var(--colorBorder);

    @media (max-width: 480px) {
      padding: 30px;
    }

    .success-message {
      text-align: center;
      padding: 40px 0;

      h5 {
        font-family: var(--fontPrimary);
        font-size: 24px;
        color: var(--colorPrimary);
        margin-bottom: 20px;
      }

      p {
        font-family: var(--fontSecondary);
        font-size: 16px;
        color: var(--colorText2);
        margin-bottom: 30px;
      }

      button {
        width: auto;
        padding: 15px 30px;
        font-size: 14px;
      }
    }

    .error-text {
      color: #ff4d4d;
      font-size: 14px;
      margin-bottom: 20px;
      font-family: var(--fontSecondary);
    }

    .form-group {
      margin-bottom: 30px;

      label {
        display: block;
        font-family: var(--fontSecondary);
        font-size: 14px;
        margin-bottom: 10px;
        color: var(--colorText2);
      }

      input,
      textarea {
        width: 100%;
        padding: 15px 0;
        background: transparent;
        border: none;
        border-bottom: 1px solid var(--colorBorder);
        font-family: var(--fontSecondary);
        font-size: 16px;
        color: var(--colorText);
        outline: none;
        transition: border-color 0.3s ease;

        &:focus {
          border-bottom-color: var(--colorPrimary);
        }
      }

      textarea {
        height: 120px;
        resize: none;
      }
    }

    button {
      width: 100%;
      padding: 20px;
      background: var(--colorPrimary);
      color: white;
      border: none;
      font-family: var(--fontSecondary);
      font-size: 16px;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 2px;
      cursor: pointer;
      transition: all 0.3s ease;

      &:hover {
        background: var(--colorText);
        transform: translateY(-5px);
      }
    }
  }
</style>
