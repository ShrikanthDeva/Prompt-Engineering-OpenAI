# Expanding
+ In this lesson, you will generate customer service emails that are tailored to each customer's review.

## Setup
```py
import openai
import os
​
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
​
openai.api_key  = os.getenv('OPENAI_API_KEY')
def get_completion(prompt, model="gpt-3.5-turbo",temperature=0): # Andrew mentioned that the prompt/ completion paradigm is preferable for this class
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=temperature, # this is the degree of randomness of the model's output
    )
    return response.choices[0].message["content"]
```
#### Customize the automated reply to a customer email
```py
# given the sentiment from the lesson on "inferring",
# and the original customer message, customize the email
sentiment = "negative"
​
# review for a blender
review = f"""
So, they still had the 17 piece system on seasonal \
sale for around $49 in the month of November, about \
half off, but for some reason (call it price gouging) \
around the second week of December the prices all went \
up to about anywhere from between $70-$89 for the same \
system. And the 11 piece system went up around $10 or \
so in price also from the earlier sale price of $29. \
So it looks okay, but if you look at the base, the part \
where the blade locks into place doesn’t look as good \
as in previous editions from a few years ago, but I \
plan to be very gentle with it (example, I crush \
very hard items like beans, ice, rice, etc. in the \ 
blender first then pulverize them in the serving size \
I want in the blender then switch to the whipping \
blade for a finer flour, and use the cross cutting blade \
first when making smoothies, then use the flat blade \
if I need them finer/less pulpy). Special tip when making \
smoothies, finely cut and freeze the fruits and \
vegetables (if using spinach-lightly stew soften the \ 
spinach then freeze until ready for use-and if making \
sorbet, use a small to medium sized food processor) \ 
that you plan to use that way you can avoid adding so \
much ice if at all-when making your smoothie. \
After about a year, the motor was making a funny noise. \
I called customer service but the warranty expired \
already, so I had to buy another one. FYI: The overall \
quality has gone done in these types of products, so \
they are kind of counting on brand recognition and \
consumer loyalty to maintain sales. Got it in about \
two days.
"""
prompt = f"""
You are a customer service AI assistant.
Your task is to send an email reply to a valued customer.
Given the customer email delimited by ```, \
Generate a reply to thank the customer for their review.
If the sentiment is positive or neutral, thank them for \
their review.
If the sentiment is negative, apologize and suggest that \
they can reach out to customer service. 
Make sure to use specific details from the review.
Write in a concise and professional tone.
Sign the email as `AI customer agent`.
Customer review: ```{review}```
Review sentiment: {sentiment}
"""
response = get_completion(prompt)
print(response)
```

```
Dear valued customer,

Thank you for taking the time to leave a review about our 17 piece system. We are sorry to hear that you experienced a price increase and that the quality of the product did not meet your expectations. We apologize for any inconvenience this may have caused you.

If you have any further concerns or questions, please do not hesitate to reach out to our customer service team. They will be more than happy to assist you in any way they can.

Thank you again for your feedback. We appreciate your business and hope to have the opportunity to serve you better in the future.

Best regards,

AI customer agent
```

#### Remind the model to use details from the customer's email

```py
prompt = f"""
You are a customer service AI assistant.
Your task is to send an email reply to a valued customer.
Given the customer email delimited by ```, \
Generate a reply to thank the customer for their review.
If the sentiment is positive or neutral, thank them for \
their review.
If the sentiment is negative, apologize and suggest that \
they can reach out to customer service. 
Make sure to use specific details from the review.
Write in a concise and professional tone.
Sign the email as `AI customer agent`.
Customer review: ```{review}```
Review sentiment: {sentiment}
"""
response = get_completion(prompt, temperature=0.7)
print(response)
```
```
Dear valued customer,

Thank you for sharing your review with us. We apologize for any inconvenience you may have experienced with the price increase and the quality of the product. We take our customers' feedback seriously and we will work on improving our products to meet your expectations.

If you have any further concerns or questions, please do not hesitate to reach out to our customer service team for assistance. We are always available to help and ensure that our customers are satisfied with their purchases.

Thank you for choosing our brand and we hope to have the opportunity to serve you better in the future.

Best regards,

AI customer agent
```