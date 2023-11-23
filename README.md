
# OpenAI Assistant API PHP SDK

A PHP class for seamless interaction with the OpenAI Assistant API, enabling developers to build powerful AI assistants capable of performing a variety of tasks. <br>

<img width="800" height="400" src="banner.png" />

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [API Reference](#api-reference)
- [Feedback](#feedback)
- [License](#license)

## Installation

Install with composer

```bash
composer require erdum/php-open-ai-assistant-sdk
```

Or install without composer on older PHP versions just clone the project in your working directory and require the OpenAIAssistant.php
```bash
git clone https://github.com/erdum/php-open-ai-assistant-sdk.git
```

```php
<?php

require(__DIR__ . 'php-open-ai-assistant-sdk/src/OpenAIAssistant.php');

use Erdum\OpenAIAssistant;

$openai = new OpenAIAssistant(
    $api_key,
    $assistant_id
);
```
    
## Usage
Currently, this usage example does not contain any information about OpenAI Assistant API working for a better understanding of how OpenAI Assistant API works and its lifecycle please refer to the [OpenAI documentation](https://platform.openai.com/docs/assistants/how-it-works) and look inside the OpenAIAssistant.php file to see all the available methods.
```php
<?php

require(__DIR__ . '/vendor/autoload.php');
# require(__DIR__ . '/php-open-ai-assistant-sdk/src/OpenAIAssistant.php');

use Erdum\OpenAIAssistant;

$openai = new OpenAIAssistant(
    $api_key,
    $assistant_id
);

$thread_id = isset($_SESSION[$session_id]) ? $_SESSION[$session_id] : null;

if (empty($thread_id)) {
    $thread_id = $openai->create_thread($query);
    $_SESSION[$session_id] = $thread_id;
} else {
    $openai->add_message($thread_id, 'Can you help me?');
}
$openai->run_thread($thread_id);

while ($openai->has_tool_calls) {
    $outputs = $openai->execute_tools(
        $thread_id,
        $openai->tool_call_id
    );
    $openai->submit_tool_outputs(
        $thread_id,
        $openai->tool_call_id,
        $outputs
    );
}

// Get the last recent message
$message = $openai->list_messages($thread_id);
$message = $message[0];
$output = '';

if ($message['role'] == 'assistant') {
    foreach ($message['content'] as $msg) {
        $output .= "{$msg['text']['value']}\n";
    }
    exit($output);
}
```
## API Reference

#### Create Assistant Object

```php
<?php

use Erdum\OpenAIAssistant;

$openai = new OpenAIAssistant($api_key);
```

| Parameter      | Type     | Description                                 |
| :------------- | :------- | :----------                                 |
| `api_key`      | `string` | **Required**. Your OpenAI API key           |
| `assistant_id` | `string` | Assistant ID (default = null)               |
| `base_url`     | `string` | OpenAI API base URL (default = 'https://api.openai.com/v1') |
| `version_header` | `string` | OpenAI API version header (default = 'OpenAI-Beta: assistants=v1' ) |

## Feedback

If you have any feedback, please reach out to us at erdumadnan@gmail.com

## License
[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)
