# Multi-factor-auth
import pyotp
from twilio.rest import Client

# Twilio credentials
account_sid = 'your_twilio_account_sid'
auth_token = 'your_twilio_auth_token'
twilio_phone_number = 'your_twilio_phone_number'
user_phone_number = '+1234567890'  # User's phone number for SMS verification

# Generate a random secret for TOTP
secret = pyotp.random_base32()

# Initialize TOTP
totp = pyotp.TOTP(secret)

# Initialize Twilio client
client = Client(account_sid, auth_token)

def generate_totp():
    # Generate TOTP for the current time
    return totp.now()

def send_sms_verification():
    # Send SMS with verification code
    verification_code = totp.now()
    message = client.messages.create(
        body=f'Your verification code is: {verification_code}',
        from_=twilio_phone_number,
        to=user_phone_number
    )
    print(f"Verification code sent to {user_phone_number}")

def verify_totp(token):
    # Verify TOTP token
    return totp.verify(token)

def main():
    # Simulate user interaction
    choice = input("Choose an option:\n1. Generate TOTP\n2. Send SMS Verification\n3. Verify TOTP\n")
    
    if choice == '1':
        print("Current TOTP:", generate_totp())
    elif choice == '2':
        send_sms_verification()
    elif choice == '3':
        token = input("Enter TOTP token: ")
        if verify_totp(token):
            print("Authentication successful!")
        else:
            print("Authentication failed.")
    else:
        print("Invalid choice")

if __name__ == "__main__":
    main()
