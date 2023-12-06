# Email Unsubscribe Notebook

This Python notebook processes emails from a specified `.mbox` file (particularly the Spam folder from a Gmail Takeout export) and sends unsubscribe requests to unique links found in these emails. It only processes emails sent within a specified number of days (default is the last 10 days)

## Project Structure

Your folder should be structured as follows:

```
❯ tree
.
├── README.md
├── Takeout
│   ├── Mail
│   │   └── Spam.mbox
│   └── archive_browser.html
├── takeout-20231206T082003Z-001.zip
└── unsub.py

```

## Prerequisites

```bash
pip install notebook requests tqdm ipywidgets
```

## Usage

1. **Set Up Your `.mbox` File**: Place your `.mbox` file in the correct directory (`Takeout/Mail/Spam.mbox`).

2. **Configure the Notebook**: Edit the `mbox_path` variable in the notebook to point to your `.mbox` file. If you want to process emails from a different time range, modify the `days` variable (set to `None` to process all emails).

3. **Run the notebook**: Open the notebook using Python.

```bash
jupyter notebook unsub.ipynb
```

   - Run each cell in the Jupyter Notebook sequentially.
   - You can execute the cells by selecting each and pressing `Shift + Enter`, or by using the 'Run' button in your Jupyter interface.

**tips, for gmail, you can use [this](https://support.google.com/accounts/answer/3024190?hl=en) to get your data**


## Notebook Overview

- `find_unsubscribe_links`: This function extracts unsubscribe links from the email content. It searches for URLs that contain the word 'unsubscribe'.

- `send_unsubscribe_request`: This function sends an HTTP GET request to each unsubscribe link. It includes a custom Firefox user-agent in the request header.

- `is_recent`: This function determines if an email is within the specified date range. It's used to filter emails by date, processing only those that were sent within the last `days` (default is 10).

- Main loop: The script iterates through the `.mbox` file, processing only recent emails based on the `days` parameter. For each email, it extracts and collects unsubscribe links, avoiding duplicates. It then uses a thread pool to send unsubscribe requests concurrently to each unique link.
