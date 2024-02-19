# Bulk_Description_generator-_with_help_of_ai
About AI Desc by SemWit

Background: SemWit Product Ranking is an advanced SEO and dynamic content wp-plugin designed for WooCommerce, Elementor and Ai Engine. It empowers site owners with AI-driven tools for bulk product description generation, dynamic linking and custom shortcode capabilities. With SemWit Product Ranking, enhance your product visibility, craft compelling content with OpenAI and AI Engine (/ or our trained models), and enjoy a new user experience with dynamic widgets and automated product rankings. Perfect for those looking to elevate their SEO game, engage users with personalized content, and harness the power of AI to stay ahead in competitive e-commerce landscapes. 

AI Desc is a central feature in this plugin, but AI Desc will be developed as a standalone plugin that can later be a part of SemWit Product Ranking.

* Plugin Name: AI Desc by SemWit
* Description: This project is all about making AI Desc. It's a standalone plugin that will later be integrated with SemWit Product Ranking. SemWit AI Desc is a WooCommerce bulk product description generator working with AI Engine.
* Version:     1.0.0
* Author:      Lars LJ
* Lisence: Will be dual licensed under the MIT and GPL licenses (http://www.opensource.org/licenses/mit-license.php) and (http://www.gnu.org/licenses/gpl.html) once distributed by SemWit.com.
* NDA: This GitHub resp, code, and business information is private between semwit and collaborators.
* Compatibility: WordPress, WooCommerce, Open AI and Ai Engine by Jordy Meow.

Purpose of the AI Desc Plugin
We want to utilize APIs to OpenAI language models to generate WooCommerce product descriptions in bulk through a custom bulk action edit product, "Generate descriptions with AI".

Key Considerations
- Current Plugin version is successfully activated, bulk action added, but the popup form / js currently doesn't popup from #doaction or #doaction2.
- Make your branch
- Feel free to make your approach to acomplish the AI Desc Plugin Purpose.
- API-Key need to be handled in a safe way to protect this user credential.
- Safety and Efficiency: Creating Nonces and handling the AJAX is essential for efficiency and safety. Feel free to implement obstruction or other safety measures.
- Currently the Plugin is designed to be copy pasted into plugin folder in FTP, but AI Desc is going to be free to install normally through WordPress, but will require a lisence before it can be activated. So needs a RAR installation for those that buy lisence on semwit.com
- In the end of the content generation process, Maybe let the user decide to Draft or Publish the descriptions.

Simplified Overview from User Perspective

1. Initiate bulk action in the WordPress dashboard under "Products."
2. Customize API calls through a user-friendly popup UI.
3. Securely store and validate the OpenAI API key.
4. Choose between OpenAI or AI Engine APIs.
5. Optionally set up a custom business profile for consistent AI language.
6. Select prompts for AI generation.
7. Monitor the automated generation process with progress updates.
8. Receive a completion notification
9. Option to Draft or Publish the descriptions.
10. Success message with a recommendation to clear cache.

How the Current Code / Process Works
Process of making descriptions for Woocommerce products
1. The user navigate to "products" in the wp dashboard and selects the products he wishes to populate with descriptions and click on the bulk action. 
2. The bulk action opens a popup window / ui, where the user can customize the API call.

The UI / Form (ai-popup.js & popup_form.php)
3. User types in API-Key (YOUR_API_KEY) which has to be safely stored. User clicks on "Validate API-Key" button which calls verify-api-key.php . If it returnes a response from OpenAI, he get a success message. Or if it's invalid, returns a error message.
4. If successfully validated the API-key the next step is populated. If not, he can try again by typing in a new API-key.
5. The user chooses what API to call. It can be two buttons; OpenAI (open-ai-api.php) or Ai Engine (ai-engine-api.php). Now the user come to the next step.
6. Do you wish to set up a custom business profile for better AI language consistency? Yes/No.
Yes: business_profiler.php / business_profiler.php comes into play. The user then types in his preferences and clicks Next.
No: The user comes to the Next step
7. The user can now choose what prompt template to use. These are supposed to be our optimized selected-prompt in prompt_templates.php . The default prompt is "id" => "Product Description".
8. In this last step the user can type in id="additional-instructions" in a text field which is passed to the selected API.
Example from ai-engine-api.php:

    $selectedModel = $MWAI_OPENAI_MODELS[$selectedModelId] ?? $MWAI_OPENAI_MODELS['gpt-3.5-turbo'];

    $generated_descriptions = [];

    foreach ($product_titles as $title) {
        $conversation = [
            ["role" => "system", "content" => $businessProfile],
            ["role" => "system", "content" => "You are a large language model trained by Open AI. Give helpful and creative responses based on the user's input. Reply in Markdown and Norwegian language."],
            ["role" => "user", "content" => "Give extensive and helpful information about $title. Ensure comprehensive coverage of the topics to the best of your ability. Follow Google's helpful content guidelines and a basic journalistic approach when you structure the articles. På norsk."]
            ["role" => "system", "content" => $selectedPromptData]
        ];

As you can see the API Call is structured with data the user has typed into our business profile aswell as our hard coded data. You can see $title is embedded in the conversation.

9. The user Submit / Generate selected descriptions. See files in /bulk-edit/ and /API/ for more details on this process.

Now, our automation process can continue with ai-description-handler.php as $api_response = wp_remote_post is an json string array returned by using our parameters too from engine_settings (for open-ai-api.php) or ai-engine-models.php for ai-engine-api.php

Extract data from AJAX request and store it in a database or table, shedule-and-update.php currently is a kron dedicated to handle the saving of descriptions. Open AI can handle batch strings, and we can recieve JSON String in a bulk, but we cant save all descriptions on their respective urls at the same time.

The user should see a process indicating "1/48 descriptions successfully saved." followed by a list of product titles with links that can open in a new tab if he want to check result as the kron job continues.

Once the job is finished and all descriptions are saved, the user get a success message window, which he can exit. It should contain a message recommending the user to clear cache.

The user has now automaticly generated WooCommerce Product Descriptions with Open AI API either through AI Engine API or directly to Open AI API.

* Current Plugin Structure *

ai-desc (root plugin folder)
│   ├── API
│   │   ├── ai-engine-api.php
│   │   └── open-ai-api.php
│   │   └── verify-api-key.php
│   ├── bulk-edit
│   │   ├── ai-desc-main.php
│   │   ├── ai-description-handler.php
│   │   ├── ai-popup.js
│   │   └── ajax-handler.php
│   │   ├── bulk-action-handler.php
│   │   ├── popup_form.php
│   │   └── schedule-and-update.php
│   ├── business-profile
│   │   ├── business-profiler.php
│   │   └── business-profile.js
│   ├── config
│   │   ├── ai-engine-models.json
│   │   ├── engine_options.json
│   │   ├── engine_settings.json
│   │   └── prompt_templates.php
│   └── ai-desc.php

Documentation

* Custom bulk action
Adding a custom bulk action: https://awhitepixel.com/wordpress-admin-add-custom-bulk-action/

* WooCommerce Product Page
Product page place holders: https://storecustomizer.com/woocommerce-product-page-hooks-visual-guide/

* APIs Documentation

1. OpenAI Documentation

2. Meow Apps Ai Engine Documentation

Tutorial: https://meowapps.com/ai-engine/tutorial/
Internal API: https://meowapps.com/ai-engine/api/
PHP functions: https://meowapps.com/ai-engine/api/#php-functions
FAQ: https://meowapps.com/ai-engine/faq/

3. WordPress REST API
REST API: https://developer.wordpress.org/rest-api/
