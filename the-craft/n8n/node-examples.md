## n8n nodes

### Webhook vs HTTP Request

* Webhook (Trigger Node)
    * Purpose: Receive data
    * Direction: External → n8n
    * When it runs: Only when something calls it

    👉 Think: “Someone sends data into my workflow”

    Example:

    * TikTok sends an event (new video stats)
    * A form submission hits your webhook URL
    * A system notifies your workflow

*  HTTP Request (Action Node)
    * Purpose: Send or fetch data
    * Direction: n8n → External API
    * When it runs: When the workflow reaches this node

    👉 Think: “My workflow requests data from somewhere else”

    Example:

    * Call TikTok API to get last week’s metrics
    * Send data to Google Sheets
    * Fetch data from any REST API  

* Simple Analogy
    * Webhook = Doorbell 🚪
        → Someone rings it, and you react
    * HTTP Request = You making a call 📞
        → You reach out to get info

### API and REST API

* API (Application Programming Interface)
    * A **set of rules** that lets two systems talk to each other
    * Defines **what you can request** and **how you must ask for it**

    👉 Think: a structured way to access someone else’s data or service

    Technical idea:
    * You send a request → API processes it → returns a response (usually JSON)

* REST API (Representational State Transfer API)
    * A **type/style of API** that follows standard web rules
    * Uses HTTP methods like:
        * GET → retrieve data
        * POST → send data
        * PUT/PATCH → update
        * DELETE → remove

    Technical traits:
    * Uses URLs (endpoints)
    * Stateless (each request is independent)
    * Common response format: JSON

    👉 Most modern APIs (including TikTok) are REST APIs

    Real-world example of a REST API: TikTok API
    * Lets you programmatically access:
        * video stats
        * user data
        * engagement metrics


* Everyday Analogy
    * API = Restaurant menu 🍽️
        → Shows **what** you can order **and how**
    * REST API = Ordering system rules
        → “Say item name + quantity” (standard way of ordering)
    * TikTok API = A specific restaurant
        → Their menu = video views, followers, etc.


