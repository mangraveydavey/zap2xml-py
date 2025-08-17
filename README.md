# zap2xml-py
A very simple script to fetch EPG data from zap2it.com and write it to XMLTV format.

[Unreleased] - 2025-06-21 Updated URLs to gracenote and resolved the June 2025 404 errors with the user agent changes.

[Unreleased] - 2025-08-16

Fixed
Improved Error Handling in get_cached:
Added handling for urllib.error.URLError to catch network issues (e.g., connection failures) and raise them with descriptive messages, enhancing script reliability.
Added check for empty responses, returning a fallback JSON structure ({"note": "Empty response, treating as 400 error, skipping.", "channels": []}) to prevent crashes.
Enhanced HTTPError handling to print error code and reason (e.g., HTTP Error: 400 - Bad Request) for improved debugging.

JSON Parsing Error Handling in main:
Added try-except block around json.loads(result) to catch json.decoder.JSONDecodeError, log the error, and skip invalid time slots with a fallback JSON structure ({"note": "Invalid JSON response, skipping.", "channels": []}) instead of crashing.
Saves non-JSON responses to response_<timestamp>.html in the cache directory for troubleshooting.

Changed
Updated Default Parameters in get_args:
Changed zap_aid default from 'gapzap' to 'orbebb' to use a more reliable affiliate ID for the Gracenote API.
Updated zap_isOverride type from bool to str with default 'true' (lowercase) to match the API’s expected query parameter format.
Changed zap_languagecode default from 'en' to 'en-us' for consistency with the API’s requirements.
Set zap_pref default from empty string ('') to '16,128' to include preferences needed for comprehensive API responses.

Enhanced Debugging Output in get_cached:
Added printing of the first 500 characters of the response (result.decode('utf-8', errors='ignore')[:500]) to aid in diagnosing server responses (e.g., JSON vs. HTML).
Prints HTTP status code and reason for errors, improving execution transparency.

Added
Response Caching for Debugging:
When JSON parsing fails, non-JSON responses are saved to response_<timestamp>.html files in the cache directory, facilitating manual inspection of server errors (e.g., HTML error pages or CAPTCHA challenges).

Notes
The script continues to use the https://tvlistings.gracenote.com/api/grid endpoint. Changes address issues with invalid affiliate IDs, incorrect parameter formats, and insufficient error handling, improving reliability for fetching TV listings.
Users can override the affiliate ID with --aid (e.g., --aid gapzap, --aid digiceltrin, --aid tribnyc2dl, --aid lat) and other parameters (e.g., --pref, --timespan) for flexibility.
If the Gracenote API introduces CAPTCHA or authentication requirements, further updates may be needed (e.g., cookie handling or Selenium for CAPTCHA).
Users encountering issues can contact Gracenote support for valid aid and lineupId values or consider SchedulesDirect for a more reliable EPG data source.

