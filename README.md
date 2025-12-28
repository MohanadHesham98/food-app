# About The Project 

**Ohana** Food Ordering Platform is a modern web application that allows users to browse, order, and manage meals online with ease. It is designed to create a seamless and user-friendly experience, both for customers and for admin management.  

**Ohana** means family — more than a restaurant, it’s about creating a sense of home, connecting loved ones, and sharing meals that bring people closer. Here, food feels like home and makes the distance fade away.

---

## Getting Started

This project uses Helm for Kubernetes deployment. You can deploy the full platform using a single Helm chart.
Follow these instructions to set up the project locally on your machine.

### Prerequisites

Make sure you have the following installed on your system:

Make sure you have the following installed:

- Node.js (v14 or later)
- npm (for package management)
- Docker and Docker Compose
- kubectl (for Kubernetes)
- Helm 3+

---

### Installation

1. **Clone the repository**:

   ```bash
   git clone https://github.com/MohanadHesham98/food-app.git
   cd food-app
   ```

2. **Build Docker images**:
     ```bash
       docker compose build
     ```
3. **Push images to Docker Hub**:
     ```bash
       docker login
       docker push mohanad898/food-backend:latest
       docker push mohanad898/food-frontend:latest
       docker push mohanad898/food-admin:latest
     ```

### Deploying on Kubernetes with Helm

1. **Navigate to the Helm chart**:
   ```bash
   cd k8s/food-app
   ```
 2. **Install or upgrade the release**:
   ```bash
   helm upgrade --install food-app ./food-app --namespace food --create-namespace
   ```

3. **Verify deployments**:
   ```bash
   kubectl get ns
   kubectl get all -n food
   kubectl get pvc -n food
   kubectl get secret -n food

   ```
   
4. **Get your machine IP**:
   ```bash
   ip a
   ```
5. **Open in browser**:
   ```bash
   http://<machine-ip>:<node-port>
   ```
## How It Works
### User Flow:
1. Sign Up: New users can create an account quickly and securely to get started with ordering.
2. Login: Existing users can log in to access their account, view past orders, and manage their profile.
3. Browse & Add to Cart: Users can explore the menu, view food details, and add desired items to their cart.
4. Apply Promocode: Users can enter a promocode at checkout to receive discounts. The platform supports two    types of coupons: percentage-based and fixed-amount discounts.
   
### Admin Dashboard:
1. Food Management: Admins can add, edit, or delete food items to keep the menu updated.
2. Promocode Management: Admins can create and remove promocodes, defining discount type and validity.
3. Order Management: Admins can monitor all orders, update their status, and ensure smooth order
   fulfillment.

## Repository Structure

The project is divided into three main parts:

1. **Frontend**:  
   The customer-facing food ordering platform is built using **React (Vite)**. This section handles user interactions, displaying food items, and managing the cart and checkout process.

2. **Backend**:  
   The server-side API, built with **Node.js** and **Express**, handles data processing, database operations, and serves requests from both the frontend and admin dashboard.

3. **Admin Dashboard**:  
   A dedicated admin interface built using **React (Vite)**, allowing admins to manage food items, orders, and promocodes. It is connected to the backend for data management and order processing.
---
### Notes
- Make sure Docker images are correctly pushed to Docker Hub before deploying via Helm.
- For any updates, rebuild images, push to Docker Hub, and then run helm upgrade --install.
- Helm simplifies deployment: no need to manually kubectl apply YAML files.





