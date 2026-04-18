# Stripe Integration Guide for REALTOR AI

## 1. Payment Setup

### 1.1 Create a Stripe Account
- Go to [Stripe](https://stripe.com) and sign up.
- Verify your email and complete the onboarding process.

### 1.2 Install Stripe Library
- Install the Stripe library using npm or yarn.
  ```bash
  npm install stripe
  ```
- Or using yarn:
  ```bash
  yarn add stripe
  ```

### 1.3 Set Up Payment Methods
- Create a payment method in your application and integrate it with Stripe.
- Example code to create a payment intent:
  ```javascript
  const stripe = require('stripe')('your-secret-key');
  const paymentIntent = await stripe.paymentIntents.create({
    amount: 1000, // Amount in cents
    currency: 'usd',
    payment_method_types: ['card'],
  });
  ```

## 2. Webhook Configuration

### 2.1 Setting Up Stripe Webhooks
- In your Stripe dashboard, navigate to Developer > Webhooks and click on "Add endpoint."
- Input your webhook URL (e.g., `https://yourdomain.com/webhook`).

### 2.2 Handling Webhook Events
- Set up your server to verify and handle events.
- Example code for handling events:
  ```javascript
  app.post('/webhook', async (req, res) => {
    const event = req.body;

    switch (event.type) {
      case 'payment_intent.succeeded':
        const intent = event.data.object; // Contains a stripe.PaymentIntent
        // Handle successful payment
        break;
      // Handle other event types
      default:
        console.log(`Unhandled event type ${event.type}`);
    }
    res.status(200).end();
  });
  ```

## 3. Subscription Management

### 3.1 Creating Subscriptions
- Offer subscription plans in your application and create a subscription:
  ```javascript
  const subscription = await stripe.subscriptions.create({
    customer: 'cus_xxx',
    items: [{plan: 'plan_xxx'}],
  });
  ```

### 3.2 Managing Subscription Status
- Handle upgrades, downgrades, and cancellations:
  ```javascript
  // Update subscription
  await stripe.subscriptions.update('sub_xxx', {items: [{id: 'item_xxx', plan: 'new_plan_xxx'}]});

  // Cancel subscription
  await stripe.subscriptions.del('sub_xxx');
  ```
