# TimeWise Specification

## 1. Feature Overview

### 1.1. Intentional Time Ownership
**Description**: TimeWise is a tool designed to support intentional time ownership. It helps users structure their day through clear, deliberate actions: starting a focus session, pausing briefly when needed, stopping to close a work block, and explicitly closing the day. Its purpose is not to capture every second automatically, but to encourage users to make conscious decisions about how they allocate their time.

### 1.2. Core Functionality
TimeWise provides a structured approach to time management through:
- Intentional timer controls (start, pause, resume, stop, reset)
- Activity management with detailed attributes
- Daily dashboard for planning and tracking
- Statistics module for insights into time allocation patterns
- User configuration for personalizing the experience

## 2. User Scenarios

### 2.1. Starting a Timer Session
**Actor**: User
**Preconditions**: 
- User has at least one activity created
- No session is currently active
**Main Flow**:
1. User selects an activity to work on
2. User clicks "Start Timer"
3. Timer begins tracking time for the selected activity
4. System displays active timer with session information
**Postconditions**:
- A new session is created for the selected activity
- Timer is active and tracking time
- Session is visible in the active timer view

### 2.2. Pausing and Resuming a Session
**Actor**: User
**Preconditions**:
- A session is currently active
**Main Flow**:
1. User clicks "Pause Timer"
2. Timer pauses and stops tracking time
3. User clicks "Resume Timer"
4. Timer resumes tracking time from where it left off
**Postconditions**:
- Session continues with paused time excluded from total duration
- Timer status reflects active or paused state

### 2.3. Stopping a Timer Session
**Actor**: User
**Preconditions**:
- A session is currently active
**Main Flow**:
1. User clicks "Stop Timer"
2. System records session end time
3. Session becomes immutable and is added to history
4. System displays session summary
**Postconditions**:
- Session is finalized and stored
- Session duration is calculated and displayed
- Session appears in activity history

### 2.4. Resetting an Unsaved Session
**Actor**: User
**Preconditions**:
- A session is active but not yet stopped
**Main Flow**:
1. User clicks "Reset Timer"
2. System prompts for confirmation
3. User confirms reset
4. Current session is discarded
**Postconditions**:
- No session data is persisted
- Timer returns to idle state

### 2.5. Closing the Day
**Actor**: User
**Preconditions**:
- User has completed all planned activities for the day
- No sessions are currently active
**Main Flow**:
1. User clicks "Close Day"
2. System finalizes all sessions for the day
3. System computes inactivity metrics
4. System ensures no sessions remain open
5. System provides reflection mechanism
**Postconditions**:
- All day's sessions are finalized
- Inactivity metrics are calculated and displayed
- Day is closed for statistical purposes

## 3. Functional Requirements

### 3.1. Intentional Timer Model
**FR-TMR-001**: The system shall provide a timer with start, pause, resume, stop, and reset functionality.

**FR-TMR-002**: When starting a timer, if another session is running, the system shall prompt the user to stop the current session first.

**FR-TMR-003**: The system shall allow pausing and resuming a session without closing it.

**FR-TMR-004**: When stopping a timer, the system shall record the session duration and make it immutable.

**FR-TMR-005**: When resetting a timer, the system shall discard the current unsaved session with confirmation.

**FR-TMR-006**: The system shall enforce session maximum duration limits with visual and auditory alerts.

**FR-TMR-007**: The system shall show warnings when total tracked time for an activity exceeds its daily limit but not auto-stop the timer.

**FR-TMR-008**: The system shall provide a close day mechanism that finalizes all sessions, computes inactivity, and ensures no sessions remain open.

### 3.2. Activity Management System
**FR-ACT-001**: The system shall allow users to create activities with labels, categories, priorities, cognitive loads, daily/session limits, descriptions, links to materials, estimated durations, deadlines, and scheduled days.

**FR-ACT-002**: The system shall allow users to update existing activities, ensuring label uniqueness and applying changes to future sessions only.

**FR-ACT-003**: The system shall allow users to archive activities to exclude them from new timer sessions while keeping them visible in history and statistics.

**FR-ACT-004**: The system shall allow users to delete activities only if no historical data is associated with them.

**FR-ACT-005**: The system shall sort and visually emphasize high-priority activities in the UI.

**FR-ACT-006**: The system shall filter and visually differentiate activities based on cognitive load intensity.

### 3.3. Daily Dashboard
**FR-DASH-001**: The system shall display a summary of today's planned workload derived from the Weekly Execution Agenda.

**FR-DASH-002**: The system shall list activities scheduled for today with total planned and executed minutes, achievement bars, and visual status indicators.

**FR-DASH-003**: The system shall display today's agenda blocks in chronological order with start/end times, activity labels, and status markers.

**FR-DASH-004**: The system shall display a progress indicator showing tracked time vs. daily work target for the current weekday.

### 3.4. Statistics Module
**FR-STAT-001**: The system shall visualize daily tracked time data with bar charts and summary tables.

**FR-STAT-002**: The system shall aggregate data over weeks or months with line or stacked bar charts.

**FR-STAT-003**: The system shall compute and display inactivity metric as `max(0, dailyWorkTarget – totalTrackedToday)`.

### 3.5. User Configuration System
**FR-CONF-001**: The system shall allow users to configure default limits for new activities.

**FR-CONF-002**: The system shall allow users to set daily work targets per weekday.

**FR-CONF-003**: The system shall allow users to choose whether the week starts on Monday or Sunday.

**FR-CONF-004**: The system shall define the user's typical weekly rhythm with weekday vs. weekend profiles.

**FR-CONF-005**: The system shall configure daily working windows with parameters like workDayStart, lunchBreakStart/End, workDayEnd, and shortBreakMinutesBetweenBlocks.

**FR-CONF-006**: The system shall ensure planned blocks respect working window boundaries.

### 3.6. Activities View
**FR-ACTVIEW-001**: The system shall display a table- or card-based list of activities with all relevant metadata.

**FR-ACTVIEW-002**: The system shall provide an edit/create form with validation feedback for required fields.

**FR-ACTVIEW-003**: The system shall support archiving activities while retaining metadata.

**FR-ACTVIEW-004**: The system shall exclude archived activities from new timer sessions but keep them visible in history and statistics.

## 4. Success Criteria

### 4.1. Usability
- Users can start, pause, resume, and stop timer sessions within 30 seconds of initiating the action
- Users can create and update activities with all attributes in under 2 minutes
- Users can navigate between dashboard, statistics, and activities views in under 10 seconds

### 4.2. Performance
- Dashboard loads within 1 second for up to 100 activities
- Statistics module displays data within 2 seconds for a full month of data
- System handles up to 100 concurrent active sessions without performance degradation

### 4.3. Reliability
- Timer sessions are never lost or corrupted
- All data integrity constraints are enforced
- System maintains consistent state across all user actions

### 4.4. Security
- All user data is stored securely with appropriate access controls
- No sensitive information is exposed in logs or error messages
- System follows OWASP security best practices

## 5. Key Entities

### 5.1. Activity
- **Label**: Unique identifier for the activity (required)
- **Category**: Professional or personal (required)
- **Priority**: Low, medium, or high (required)
- **Cognitive Load**: Light, moderate, or intense (required)
- **Daily Maximum**: Minutes per day (required)
- **Session Maximum**: Minutes per session (required)
- **Description**: Free text description (optional)
- **Estimated Duration**: Target duration in minutes (optional)
- **Deadline**: Date when activity should be completed (optional)
- **Scheduled Days**: Days of the week when activity is planned (required)
- **Archived**: Boolean indicating if activity is archived (required)

### 5.2. Session
- **Session Start**: Timestamp when session began (required)
- **Session End**: Timestamp when session ended (optional)
- **Intervals**: Time periods during which activity was tracked (required)
- **Total Duration**: Calculated duration of the session (required)

### 5.3. Interval
- **Start**: Timestamp when interval began (required)
- **End**: Timestamp when interval ended (optional)
- **Duration**: Calculated duration of the interval (required)

## 6. Assumptions

- Users will have basic computer literacy and familiarity with time tracking concepts
- The system will be used primarily on desktop devices
- Users will create activities before starting timer sessions
- All time tracking will be done in a single time zone
- Users will have a consistent daily schedule for planning purposes

## 7. Dependencies

- Data model defined in TimeWise_DataModel.md
- User configuration system for global settings
- Statistics engine for data visualization
- Timer engine for session tracking