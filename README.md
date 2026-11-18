# Ruzuku MCP server

Connect Ruzuku to ChatGPT, Claude and any AI agent through Cotlet.dev. The hosted integration documents 29 operations for course dashboards, cohorts, enrollments, assignments, meetings and messages. By default, 21 write operations ask for approval.

## What you can ask

> Summarize revenue, enrollments and pending assignments for my course.

> List my students and their course progress.

> Draft feedback on pending assignments.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, with connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings → Connectors → Add custom connector, with URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Ruzuku tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 ruzuku_agent.py serve
```

Or with Docker:

```bash
docker build -t ruzuku-mcp .
docker run -i --rm -e COTLET_API_KEY ruzuku-mcp
```

Tools: `ruzuku_guide`, `ruzuku_status`, `ruzuku_open`, `ruzuku_read`, `ruzuku_click`, `ruzuku_type`, `ruzuku_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 ruzuku_agent.py connect   # ready config for ChatGPT, Claude, Claude Code and Cursor
python3 ruzuku_agent.py status    # is Ruzuku ready in your Cotlet.dev account
python3 ruzuku_agent.py guide     # Ruzuku screens and sign-in link
python3 ruzuku_agent.py read      # open Ruzuku, read the page, close the browser
python3 ruzuku_agent.py tools     # list Cotlet.dev tools
python3 ruzuku_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Ruzuku users

**Where do I sign in to Ruzuku?**

Open https://app.ruzuku.com in your Cotlet Cloud Browser and sign in with your own instructor or admin account.

**Which changes ask for approval?**

By default, 21 of the 24 write operations ask first. Creating courses, building outlines and creating quizzes run without approval. The five reporting operations read data.

**Is Cotlet affiliated with Ruzuku?**

No. Cotlet.dev is independent and not affiliated with Ruzuku.

## Learn more

- [Ruzuku integration and hosted tools](https://cotlet.dev/integrations/ruzuku-api-mcp-server)

---

More: https://cotlet.dev/integrations/ruzuku-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=ruzuku

Request access: https://cotlet.dev/signup?app=ruzuku&utm_source=github&utm_medium=readme&utm_campaign=ruzuku

## Workflows on the hosted integration

The live integration page documents these 29 operations:

- `view_course_dashboard`
- `list_students`
- `export_student_data`
- `download_meeting_recording`
- `export_course_backup`
- `create_course`
- `archive_course`
- `build_course_outline`
- `delete_module_or_lesson`
- `add_lesson_content`
- `create_quiz`
- `create_discussion_forum`
- `customize_course_style`
- `edit_sales_page`
- `manage_price_points`
- `create_coupon`
- `create_bundle`
- `add_checkout_upsell`
- `create_digital_product`
- `set_enrollment`
- `invite_students`
- `bulk_enroll_students`
- `refund_student`
- `remove_student`
- `add_facilitator`
- `schedule_course_message`
- `schedule_meeting`
- `review_assignments`
- `send_chat_message`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
