
# OpenAI Assistant API PHP SDK

A PHP class for seamless interaction with the OpenAI Assistant API, enabling developers to build powerful AI assistants capable of performing a variety of tasks. <br>

<img width="600" height="400" src="banner.png" />

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
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
```
    
## Usage
Currently, this usage example does not contain any information about OpenAI Assistant API working for a better understanding of how OpenAI Assistant API works and its lifecycle please refer to the [OpenAI documentation](https://platform.openai.com/docs/assistants/how-it-works) and look inside the OpenAIAssistant.php file to see all the available methods.
```php
<?php

$thread_id = $_SESSION[$session_id];

try {
    $openai = new OpenAIAssistant(
        OpenAI-API-Key,
        Assistant-ID
    );

    if (empty($thread_id)) {
        $thread_id = $openai->create_thread();
        $_SESSION[$session_id] = $thread_id;
    }

    $message_id = $openai->create_message($thread_id, 'I want to check my account balance');

    if ($message_id) $openai->run_thread($thread_id);

    if ($openai->has_tool_calls) {
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
} catch (Exception $err) {
    echo($err);
}
```
## Feedback

If you have any feedback, please reach out to us at erdumadnan@gmail.com

## License
[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)
