# Overview
The Dad Joke REST API enables you to access a vast database of dad jokes to obtain:

* A random dad joke
* A random dad joke formatted for Slack
* A specific dad joke
* A dad joke as an image
* A list of dad jokes

## Resource details
The Dad Jokes API is located at https://icanhazdadjoke.com.

No authentication is required to use the Dad Jokes API.

In this tutorial the curl command is used for Dad Jokes API requests and the JSON format for the response output.

**Note**: Python, JavaScript, Java, Ruby, and other popular programming languages can be used to make HTTP requests to the Dad Joke API endpoints; however, these languages are out of the scope of and not included in this tutorial.

## Header format
Valid Accept headers include:
* `text/html` - HTML response (default response format)
* `application/json` - JSON response
* `text/plain` - Plain text response

**Note**: `curl` requests made without a valid `Accept` header receive a `text/plain` response.

## Custom user agent
The Dad Joke API requires that you set a custom `User-Agent` header for all requests. By setting a custom `User-Agent` header for your code you facilitate usage monitoring.

Your user agent should include the name of the library or website that is accessing the API along with a contact URL or e-email.

For example:
```
$ curl -H "User-Agent: My Library (https://github.com/username/repo)" https://icanhazdadjoke.com/
```
### Dad Joke requests
The following sections describe some of the requests you can make to the Dad Joke endpoint:

* [Obtain a random dad joke]{}
* Obtain a random dad joke as a Slack message
* Obtain a specific dad joke
* Obtain a dad joke as an image
* Search for dad jokes
* Graph QL query

## Obtain a random dad joke
To obtain a random dad joke, use the following command:
```
$ curl -H "Accept: application/json" https://icanhazdadjoke.com
```
Response:
```
{
  "id": "R7UfaahVfFd",
  "joke": "My dog used to chase people on a bike a lot. It got so bad I had to take his bike away.",
  "status": 200
}
```

## Obtain a random dad joke formatted for Slack
To obtain a random dad joke formatted for Slack, use the following command:
```
$ curl -H "Accept: application/json" https://icanhazdadjoke.com/slack.
```
Response:
```
{
  "attachments": [
    {
      "fallback": "What kind of magic do cows believe in? MOODOO.",
      "footer": " - ",
      "text": "What kind of magic do cows believe in? MOODOO."
    }
  ],
  "response_type": "in_channel",
  "username": "icanhazdadjoke"
}
```
## Obtain a specific dad joke
To obtain a specific Dad Joke you use the `<joke_id>` parameter in the following request format:

```
$ curl -H "Accept: application/json" https://icanhazdadjoke.com/j/<joke_id>
```
Using the `joke_id` value of `R7UfaahVfFd` shown in a previous example you make the following request:
```
$ curl -H "Accept: application/json" https://icanhazdadjoke.com/j/R7UfaahVfFd 
```
Response:
```
{
  "id": "R7UfaahVfFd",
  "joke": "My dog used to chase people on a bike a lot. It got so bad I had to take his bike away.",
  "status": 200
}
```
## Obtain a dad joke as an image
To obtain a dad joke as an image, append the `<joke_id>` parameter with `.png` and make the following request:
```
$ curl -H "Accept: application/json" https://icanhazdadjoke.com/j/<joke_id>.png
```
Using the `joke_id` value of `R7UfaahVfFd` shown in a previous example you make the following request:
```
The response example:
```
<img src="https://icanhazdadjoke.com/j/R7UfaahVfFd.png" />
```
 
## Search for dad jokes
You can search for a specific range of dad jokes using the following query string parameters appended to the search parameter on the Dad Joke API URL:
* `page`: The page of results to obtain (the default value is page 1)
* `limit`: The number of results to return per page (the default number of results to return per page is 20 with a maximum of 30)
* `term`: The search term to use (the default is to list all jokes)

For example appending the Dad Joke URL with `search` and the `term` parameter in the following:
```
$ curl -H "Accept: application/json" https://icanhazdadjoke.com/search?term=hipster”
```
Provides the following JSON response output:
```
{
  "current_page": 1,
  "limit": 20,
  "next_page": 1,
  "previous_page": 1,
  "results": [
    {
      "id": "GlGBIY0wAAd",
      "joke": "How much does a hipster weigh? An instagram."
    },
    {
      "id": "xc21Lmbxcib",
      "joke": "How did the hipster burn the roof of his mouth? He ate the pizza before it was cool."
    },
    {
      "id": "NRuHJYgaUDd",
      "joke": "How many hipsters does it take to change a lightbulb? Oh, it's a really obscure number. You've probably never heard of it."
    }
  ],
  "search_term": "hipster",
  "status": 200,
  "total_jokes": 3,
  "total_pages": 1
}
```
### Pagination
You can control pagination using the page and limit parameters. 
For example, to see a limit of 10 dad jokes on the 3rd page of results you use the following command:
```
$ curl -H "Accept: application/json" "https://icanhazdadjoke.com/?page=3&limit=10"
```
Which generates the the following response showing all of the dad jokes on the 3rd page of results:
```
{"id":"FdN7wcxAskb","joke":"They're making a movie about clocks. It's about time","status":200}
```
## Obtain a joke by GraphQL query
You can obtain a specific joke and its text using a POST request sent to the Dad Joke API GraphQL endpoint.

An example of the full curl command is as follows:
```
$ curl -X POST -d '{"query": "query { joke {id joke permalink } }"}' -H "Content-Type: application/json" https://icanhazdadjoke.com/graphql
```
The elements of note in this request are:

* `-d '{"query": "query { joke {id joke permalink } }"}'`: This option sends data in the body of the `POST` request. The data being sent is a JSON string containing a GraphQL query. The query requests a joke with its ID, the joke itself, and its permalink.
* `-H "Content-Type: application/json"`: This option sets the content type of the request header to JSON. It tells the server that the data being sent in the body is JSON formatted.
* `https://icanhazdadjoke.com/graphql`: This is the URL to which the `POST` request is being sent. It's a GraphQL endpoint hosted at `https://icanhazdadjoke.com`.

After you have entered the curl command:
1. `curl` sends an `HTTP POST` request to `https://icanhazdadjoke.com/graphql`.
2. The body of the `POST` request contains the GraphQL query, specifying that you want to retrieve a joke with its ID, the joke text itself, and its permalink.
3. The server processes the request, executes the GraphQL query, and returns the requested data.
4. `curl` displays the response from the server in your terminal. This response is in JSON format, containing the requested joke data.

The following is a JSON output example for this type of request:
```
{"data":{"joke":{"id":"3E6U8UKmbFd","joke":"Sometimes I tuck my knees into my chest and lean forward.  That\u2019s just how I roll.","permalink":"https://icanhazdadjoke.com/j/3E6U8UKmbFd"}}}%
```
# Error Handling
Successful requests to the Dad Jokes API provide the requested dad joke and associated details along with a `“status”: 200` message.

Malformed or incomplete requests receive an error message specifying the issue or problem with the request.

For example:
```
curl: (3) URL rejected: Malformed input to a URL function
```


