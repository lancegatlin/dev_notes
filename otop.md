# Install
brew install oath-toolkit

# Generate current TOTP code (6 digits)
oathtool --totp --base32 "YOUR_SECRET_KEY"
That's it — outputs the current 6-digit code.

Useful flags:
- -d 8 — if you need 8-digit codes instead of 6
- --window 1 — show previous/current/next codes (useful if timing is tight)

