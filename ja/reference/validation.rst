バリデーション
==============

:doc:`Phalcon\\Validation <../api/Phalcon_Validation>` は任意のデータセットを検証する、独立バリデーションコンポーネントです。
このコンポーネントは、モデルまたはコレクションに属していないデータオブジェクトの検証ルールを実装するために使用することができます。

次の例は基本的な使い方です。

.. code-block:: php

    <?php

    use Phalcon\Validation;
    use Phalcon\Validation\Validator\Email;
    use Phalcon\Validation\Validator\PresenceOf;

    $validation = new Validation();

    $validation->add(
        "name",
        new PresenceOf(
            [
                "message" => "The name is required",
            ]
        )
    );

    $validation->add(
        "email",
        new PresenceOf(
            [
                "message" => "The e-mail is required",
            ]
        )
    );

    $validation->add(
        "email",
        new Email(
            [
                "message" => "The e-mail is not valid",
            ]
        )
    );

    $messages = $validation->validate($_POST);

    if (count($messages)) {
        foreach ($messages as $message) {
            echo $message, "<br>";
        }
    }

このコンポーネントの疎結合な設計思想は、フレームワークによって提供されているバリデータを使って、あなたが自分自身のバリデータを作ることを可能にしています。

バリデーションの初期化
-----------------------
バリデーションチェーンは :doc:`Phalcon\\Validation <../api/Phalcon_Validation>` オブジェクトにバリデータを追加するという直接的な方法で初期化することができます。
You can put your validations in a separate file for better re-use code and organization:

.. code-block:: php

    <?php

    use Phalcon\Validation;
    use Phalcon\Validation\Validator\Email;
    use Phalcon\Validation\Validator\PresenceOf;

    class MyValidation extends Validation
    {
        public function initialize()
        {
            $this->add(
                "name",
                new PresenceOf(
                    [
                        "message" => "The name is required",
                    ]
                )
            );

            $this->add(
                "email",
                new PresenceOf(
                    [
                        "message" => "The e-mail is required",
                    ]
                )
            );

            $this->add(
                "email",
                new Email(
                    [
                        "message" => "The e-mail is not valid",
                    ]
                )
            );
        }
    }

Then initialize and use your own validator:

.. code-block:: php

    <?php

    $validation = new MyValidation();

    $messages = $validation->validate($_POST);

    if (count($messages)) {
        foreach ($messages as $message) {
            echo $message, "<br>";
        }
    }

ビルトイン バリデータ一覧
-------------------------
Phalcon exposes a set of built-in validators for this component:

+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| Class                                                                                                  | Explanation                                                       |
+========================================================================================================+===================================================================+
| :doc:`Phalcon\\Validation\\Validator\\Alnum <../api/Phalcon_Validation_Validator_Alnum>`               | Validates that a field's value is only alphanumeric character(s). |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\Alpha <../api/Phalcon_Validation_Validator_Alpha>`               | Validates that a field's value is only alphabetic character(s).   |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\Date <../api/Phalcon_Validation_Validator_Date>`                 | Validates that a field's value is a valid date.                   |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\Digit <../api/Phalcon_Validation_Validator_Digit>`               | Validates that a field's value is only numeric character(s).      |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\File <../api/Phalcon_Validation_Validator_File>`                 | Validates that a field's value is a correct file.                 |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\Uniqueness <../api/Phalcon_Validation_Validator_Uniqueness>`     | Validates that a field's value is unique in the related model.    |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\Numericality <../api/Phalcon_Validation_Validator_Numericality>` | Validates that a field's value is a valid numeric value.          |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\PresenceOf <../api/Phalcon_Validation_Validator_PresenceOf>`     | Validates that a field's value is not null or empty string.       |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\Identical <../api/Phalcon_Validation_Validator_Identical>`       | Validates that a field's value is the same as a specified value   |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\Email <../api/Phalcon_Validation_Validator_Email>`               | Validates that field contains a valid email format                |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\ExclusionIn <../api/Phalcon_Validation_Validator_ExclusionIn>`   | Validates that a value is not within a list of possible values    |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\InclusionIn <../api/Phalcon_Validation_Validator_InclusionIn>`   | Validates that a value is within a list of possible values        |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\Regex <../api/Phalcon_Validation_Validator_Regex>`               | Validates that the value of a field matches a regular expression  |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\StringLength <../api/Phalcon_Validation_Validator_StringLength>` | Validates the length of a string                                  |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\Between <../api/Phalcon_Validation_Validator_Between>`           | Validates that a value is between two values                      |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\Confirmation <../api/Phalcon_Validation_Validator_Confirmation>` | Validates that a value is the same as another present in the data |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\Url <../api/Phalcon_Validation_Validator_Url>`                   | Validates that field contains a valid URL                         |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\CreditCard <../api/Phalcon_Validation_Validator_CreditCard>`     | Validates a credit card number                                    |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
| :doc:`Phalcon\\Validation\\Validator\\Callback <../api/Phalcon_Validation_Validator_Callback>`         | Validates using callback function                                  |
+--------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+

The following example explains how to create additional validators for this component:

.. code-block:: php

    <?php

    use Phalcon\Validation;
    use Phalcon\Validation\Message;
    use Phalcon\Validation\Validator;

    class IpValidator extends Validator
    {
        /**
         * Executes the validation
         *
         * @param Phalcon\Validation $validator
         * @param string $attribute
         * @return boolean
         */
        public function validate(Validation $validator, $attribute)
        {
            $value = $validator->getValue($attribute);

            if (!filter_var($value, FILTER_VALIDATE_IP, FILTER_FLAG_IPV4 | FILTER_FLAG_IPV6)) {
                $message = $this->getOption("message");

                if (!$message) {
                    $message = "The IP is not valid";
                }

                $validator->appendMessage(
                    new Message($message, $attribute, "Ip")
                );

                return false;
            }

            return true;
        }
    }

It is important that validators return a valid boolean value indicating if the validation was successful or not.

Callback Validator
------------------
By using :doc:`Phalcon\\Validation\\Validator\\Callback <../api/Phalcon_Validation_Validator_Callback>` you can execute custom
function which must return boolean or new validator class which will be used to validate the same field. By returning :code:`true`
validation will be successful, returning :code:`false` will mean validation failed. When executing this validator Phalcon will pass
data depending what it is - if it's an entity then entity will be passed, otherwise data. There is example:

.. code-block:: php

    <?php

    use \Phalcon\Validation;
    use \Phalcon\Validation\Validator\Callback;
    use \Phalcon\Validation\Validator\PresenceOf;

    $validation = new Validation();
    $validation->add(
        "amount",
        new Callback(
            [
                "callback" => function($data) {
                    return $data["amount"] % 2 == 0;
                },
                "message" => "Only even number of products are accepted"
            ]
        )
    );
    $validation->add(
        "amount",
        new Callback(
            [
                "callback" => function($data) {
                    if($data["amount"] % 2 == 0) {
                        return $data["amount"] != 2;
                    }

                    return true;
                },
                "message" => "You can't buy 2 products"
            ]
        )
    );
    $validation->add(
        "description",
        new Callback(
            [
                "callback" => function($data) {
                    if($data["amount"] >= 10) {
                        return new PresenceOf(
                            [
                                "message" => "You must write why you need so big amount."
                            ]
                        );
                    }

                    return true;
                }
            ]
        )
    );

    $messages = $validation->validate(["amount" => 1]); // will return message from first validator
    $messages = $validation->validate(["amount" => 2]); // will return message from second validator
    $messages = $validation->validate(["amount" => 10]); // will return message from validator returned by third validator

バリデーションメッセージ
------------------------
:doc:`Phalcon\\Validation <../api/Phalcon_Validation>` has a messaging subsystem that provides a flexible way to output or store the
validation messages generated during the validation processes.

Each message consists of an instance of the class :doc:`Phalcon\\Validation\\Message <../api/Phalcon_Mvc_Model_Message>`. The set of
messages generated can be retrieved with the :code:`getMessages()` method. Each message provides extended information like the attribute that
generated the message or the message type:

.. code-block:: php

    <?php

    $messages = $validation->validate();

    if (count($messages)) {
        foreach ($messages as $message) {
            echo "Message: ", $message->getMessage(), "\n";
            echo "Field: ", $message->getField(), "\n";
            echo "Type: ", $message->getType(), "\n";
        }
    }

You can pass a 'message' parameter to change/translate the default message in each validator:

.. code-block:: php

    <?php

    use Phalcon\Validation\Validator\Email;

    $validation->add(
        "email",
        new Email(
            [
                "message" => "The e-mail is not valid",
            ]
        )
    );

By default, the :code:`getMessages()` method returns all the messages generated during validation. You can filter messages
for a specific field using the :code:`filter()` method:

.. code-block:: php

    <?php

    $messages = $validation->validate();

    if (count($messages)) {
        // Filter only the messages generated for the field 'name'
        $filteredMessages = $messages->filter("name");

        foreach ($filteredMessages as $message) {
            echo $message;
        }
    }

データのフィルタリング
----------------------
Data can be filtered prior to the validation ensuring that malicious or incorrect data is not validated.

.. code-block:: php

    <?php

    use Phalcon\Validation;

    $validation = new Validation();

    $validation->add(
        "name",
        new PresenceOf(
            [
                "message" => "The name is required",
            ]
        )
    );

    $validation->add(
        "email",
        new PresenceOf(
            [
                "message" => "The email is required",
            ]
        )
    );

    // Filter any extra space
    $validation->setFilters("name", "trim");
    $validation->setFilters("email", "trim");

Filtering and sanitizing is performed using the :doc:`filter <filter>` component. You can add more filters to this
component or use the built-in ones.

バリデーション・イベント
------------------------
When validations are organized in classes, you can implement the :code:`beforeValidation()` and :code:`afterValidation()` methods to perform additional checks, filters, clean-up, etc. If the :code:`beforeValidation()` method returns false the validation is automatically
cancelled:

.. code-block:: php

    <?php

    use Phalcon\Validation;

    class LoginValidation extends Validation
    {
        public function initialize()
        {
            // ...
        }

        /**
         * Executed before validation
         *
         * @param array $data
         * @param object $entity
         * @param Phalcon\Validation\Message\Group $messages
         * @return bool
         */
        public function beforeValidation($data, $entity, $messages)
        {
            if ($this->request->getHttpHost() !== "admin.mydomain.com") {
                $messages->appendMessage(
                    new Message("Only users can log on in the administration domain")
                );

                return false;
            }

            return true;
        }

        /**
         * Executed after validation
         *
         * @param array $data
         * @param object $entity
         * @param Phalcon\Validation\Message\Group $messages
         */
        public function afterValidation($data, $entity, $messages)
        {
            // ... Add additional messages or perform more validations
        }
    }

バリデーションのキャンセル
--------------------------
By default all validators assigned to a field are tested regardless if one of them have failed or not. You can change
this behavior by telling the validation component which validator may stop the validation:

.. code-block:: php

    <?php

    use Phalcon\Validation;
    use Phalcon\Validation\Validator\Regex;
    use Phalcon\Validation\Validator\PresenceOf;

    $validation = new Validation();

    $validation->add(
        "telephone",
        new PresenceOf(
            [
                "message"      => "The telephone is required",
                "cancelOnFail" => true,
            ]
        )
    );

    $validation->add(
        "telephone",
        new Regex(
            [
                "message" => "The telephone is required",
                "pattern" => "/\+44 [0-9]+/",
            ]
        )
    );

    $validation->add(
        "telephone",
        new StringLength(
            [
                "messageMinimum" => "The telephone is too short",
                "min"            => 2,
            ]
        )
    );

The first validator has the option 'cancelOnFail' with a value of true, therefore if that validator fails the remaining
validators in the chain are not executed.

If you are creating custom validators you can dynamically stop the validation chain by setting the 'cancelOnFail' option:

.. code-block:: php

    <?php

    use Phalcon\Validation;
    use Phalcon\Validation\Message;
    use Phalcon\Validation\Validator;

    class MyValidator extends Validator
    {
        /**
         * Executes the validation
         *
         * @param Phalcon\Validation $validator
         * @param string $attribute
         * @return boolean
         */
        public function validate(Validation $validator, $attribute)
        {
            // If the attribute value is name we must stop the chain
            if ($attribute === "name") {
                $this->setOption("cancelOnFail", true);
            }

            // ...
        }
    }

Avoid validate empty values
---------------------------
You can pass the option 'allowEmpty' to all the built-in validators to avoid the validation to be performed if an empty value is passed:

.. code-block:: php

    <?php

    use Phalcon\Validation;
    use Phalcon\Validation\Validator\Regex;

    $validation = new Validation();

    $validation->add(
        "telephone",
        new Regex(
            [
                "message"    => "The telephone is required",
                "pattern"    => "/\+44 [0-9]+/",
                "allowEmpty" => true,
            ]
        )
    );

Recursive Validation
--------------------
You can also run Validation instances within another via the :code:`afterValidation()` method. In this example, validating the CompanyValidation instance will also check the PhoneValidation instance:

.. code-block:: php

    <?php

    use Phalcon\Validation;

    class CompanyValidation extends Validation
    {
        /**
         * @var PhoneValidation
         */
        protected $phoneValidation;



        public function initialize()
        {
            $this->phoneValidation = new PhoneValidation();
        }



        public function afterValidation($data, $entity, $messages)
        {
            $phoneValidationMessages = $this->phoneValidation->validate(
                $data["phone"]
            );

            $messages->appendMessages(
                $phoneValidationMessages
            );
        }
    }
