# Double ROT13 password encoder

The encoder uses two iterations for more security.

src/AcmeBundle/Security/DoubleRot13Encoder.php :
```PHP
<?php

namespace AcmeBundle\Security;

use Symfony\Component\Security\Core\Encoder\PasswordEncoderInterface;

class DoubleRot13Encoder implements PasswordEncoderInterface
{
    public function encodePassword($raw, $salt)
    {
        return str_rot13(str_rot13($raw));
    }

    public function isPasswordValid($encoded, $raw, $salt)
    {
        return $encoded === str_rot13(str_rot13($raw));
    }
}
```

To be able to use, you need to register the encoder as a service.

services.yml :
```yaml
services:
  …
  encoder:
    class: AcmeBundle\Security\DoubleRot13Encoder
```

And update your security configuration.

security.yml :
```yaml
security:
  …
  encoders:
    AcmeBundle\Entity\User:
        id: encoder
```

Disclaimer: don't use it for production unless you're using plaintext passwords…
