# Feature Specification: Intentional Timer Model

**Feature Branch**: `001-intentional-timer`  
**Created**: December 21, 2025  
**Status**: Draft  
**Input**: User description: "## 1.1. Intentional Timer Model
Description: Core functionality for managing work sessions with clear boundaries and limits.

Start Timer: Begin a new session; if another is running, prompt to stop it first (with confirmation).
Pause/Resume: Temporarily pause a session without closing it, intended for short interruptions within the same session.
Stop Timer: Finalize the current session, recording its duration and making it immutable.
Reset Timer: Discard the current unsaved session with confirmation; does not persist changes.
Enforced Limits:
SessionMax: Auto-stops a session when its configured maximum duration is reached, with visual and auditory alerts.
DailyMax: Shows warnings when the total tracked time for an activity exceeds its daily limit but doesn't auto-stop the timer.
Close Day: Finalizes all sessions for the day, computes inactivity, and ensures no sessions remain open. Provides a behavioural closure mechanism for reflection. Ensures deterministic statistical outputs for each day. Use the existing datamodel."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Start Timer (Priority: P1)

Users want to begin tracking time for an activity with clear boundaries and limits.

**Why this priority**: This is the core functionality of the timer system and enables all other timer operations.

**Independent Test**: Can be fully tested by starting a timer and verifying that a new session is created with correct activity reference and start time.

**Acceptance Scenarios**:

1. **Given** no active session exists, **When** user clicks "Start Timer", **Then** a new session is created for the selected activity
2. **Given** an active session exists, **When** user clicks "Start Timer", **Then** user is prompted to stop the current session first
3. **Given** user confirms stopping the current session, **When** user clicks "Start Timer", **Then** a new session is created for the selected activity

---

### User Story 2 - Pause/Resume Timer (Priority: P2)

Users want to temporarily pause a session without closing it, for short interruptions within the same session.

**Why this priority**: This enables users to handle interruptions without losing their tracking progress.

**Independent Test**: Can be tested by starting a timer, pausing it, resuming it, and verifying that intervals are correctly recorded.

**Acceptance Scenarios**:

1. **Given** an active session exists, **When** user clicks "Pause Timer", **Then** the session is paused and no new intervals are recorded
2. **Given** a paused session exists, **When** user clicks "Resume Timer", **Then** the session resumes and new intervals are recorded
3. **Given** a paused session exists, **When** user clicks "Pause Timer" again, **Then** no action occurs (timer already paused)

---

### User Story 3 - Stop Timer (Priority: P2)

Users want to finalize a session, recording its duration and making it immutable.

**Why this priority**: This is essential for completing time tracking and ensuring data integrity.

**Independent Test**: Can be tested by starting a timer, running it for a period, stopping it, and verifying that the session is finalized with correct duration.

**Acceptance Scenarios**:

1. **Given** an active session exists, **When** user clicks "Stop Timer", **Then** the session is finalized with a sessionEnd timestamp
2. **Given** a session has been stopped, **When** user attempts to modify it, **Then** the system prevents modifications
3. **Given** a session has been stopped, **When** user views session details, **Then** the total duration is displayed correctly

---

### User Story 4 - Reset Timer (Priority: P3)

Users want to discard the current unsaved session with confirmation.

**Why this priority**: This provides a safety mechanism for users who accidentally start a timer.

**Independent Test**: Can be tested by starting a timer, resetting it, and verifying that no session is created.

**Acceptance Scenarios**:

1. **Given** an active session exists, **When** user clicks "Reset Timer", **Then** user is prompted for confirmation
2. **Given** user confirms reset, **When** user clicks "Reset Timer", **Then** the current session is discarded
3. **Given** user cancels reset, **When** user clicks "Reset Timer", **Then** the current session remains active

---

### User Story 5 - SessionMax Enforcement (Priority: P1)

Users want the system to automatically stop sessions when they exceed configured maximum duration.

**Why this priority**: This enforces time limits and prevents users from exceeding their planned time allocation.

**Independent Test**: Can be tested by setting a short session max, starting a timer, and verifying that it stops automatically.

**Acceptance Scenarios**:

1. **Given** an activity has a sessionMax configured, **When** session duration reaches sessionMax, **Then** session is automatically stopped
2. **Given** session is automatically stopped, **When** user views session, **Then** visual and auditory alerts are displayed
3. **Given** session is automatically stopped, **When** user views session, **Then** sessionEnd is set and duration is recorded

---

### User Story 6 - DailyMax Warning (Priority: P2)

Users want to be warned when tracked time for an activity exceeds its daily limit.

**Why this priority**: This helps users stay within their planned daily time allocation without interrupting their workflow.

**Independent Test**: Can be tested by setting up an activity with a daily limit, tracking time, and verifying warnings appear.

**Acceptance Scenarios**:

1. **Given** an activity has a dailyMax configured, **When** tracked time exceeds dailyMax, **Then** warning is displayed to user
2. **Given** warning is displayed, **When** user continues tracking, **Then** session continues but warning persists
3. **Given** user has exceeded dailyMax, **When** user views activity details, **Then** dailyMax usage is clearly indicated

---

### User Story 7 - Close Day (Priority: P1)

Users want to finalize all sessions for the day, compute inactivity, and ensure no sessions remain open.

**Why this priority**: This provides a behavioral closure mechanism and ensures deterministic statistical outputs.

**Independent Test**: Can be tested by closing a day and verifying that all sessions are finalized and inactivity is computed.

**Acceptance Scenarios**:

1. **Given** sessions exist for the day, **When** user clicks "Close Day", **Then** all open sessions are finalized
2. **Given** sessions are finalized, **When** user clicks "Close Day", **Then** inactivity duration is computed for the day
3. **Given** day is closed, **When** user attempts to start a new session, **Then** system prevents new sessions for that day

---

### Edge Cases

- What happens when a session spans across midnight? 
- How does system handle multiple concurrent timers?
- What happens when a user tries to pause a non-existent timer?
- How does system handle session max limits when the timer is paused?
- What happens when a user closes a day with no sessions?
- How does system handle time zone changes during active sessions?

## Requirements *(mandatory)*

### Functional Requirements

- **FR-TMR-001**: System MUST allow users to start a new timer session for an activity
- **FR-TMR-002**: System MUST prompt users to stop an existing session before starting a new one
- **FR-TMR-003**: System MUST support pausing and resuming active timer sessions
- **FR-TMR-004**: System MUST allow users to stop a timer session and finalize it
- **FR-TMR-005**: System MUST allow users to reset an unsaved timer session with confirmation
- **FR-TMR-006**: System MUST automatically stop timer sessions when they reach configured sessionMax duration
- **FR-TMR-007**: System MUST display warnings when tracked time for an activity exceeds its dailyMax
- **FR-TMR-008**: System MUST finalize all sessions for a day when user clicks "Close Day"
- **FR-TMR-009**: System MUST compute and store inactivity duration for closed days
- **FR-TMR-010**: System MUST prevent new sessions from being started for closed days
- **FR-TMR-011**: System MUST ensure that session intervals are non-overlapping and chronological
- **FR-TMR-012**: System MUST derive session totalDuration from the sum of all intervals
- **FR-TMR-013**: System MUST validate that sessionStart and sessionEnd timestamps are valid epoch milliseconds
- **FR-TMR-014**: System MUST ensure that sessions do not span across calendar days
- **FR-TMR-015**: System MUST ensure that only one session can be active at any time
- **FR-TMR-016**: System MUST ensure that paused time does not contribute to session totalDuration

### Key Entities *(include if feature involves data)*

- **Session**: Represents a time tracking session for an activity. Key attributes include: id, activityId, sessionStart, sessionEnd, intervals, totalDuration.
- **Interval**: Represents a time span within a session. Key attributes include: start, end, duration.
- **Activity**: Represents a tracked activity with configured limits. Key attributes include: id, dailyMax, sessionMax.
- **DaySnapshot**: Represents per-day metadata for statistics and inactivity computation. Key attributes include: date, firstTimerAt, dayEndAt, inactivityDurationMs.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-TMR-001**: Users can start, pause, resume, and stop timer sessions in under 2 seconds
- **SC-TMR-002**: System automatically stops sessions when they reach sessionMax duration with visual and auditory alerts
- **SC-TMR-003**: System displays dailyMax warnings when tracked time exceeds daily limits
- **SC-TMR-004**: System finalizes all sessions for a day when "Close Day" is clicked
- **SC-TMR-005**: System computes inactivity duration for closed days with 95% accuracy
- **SC-TMR-006**: System prevents new sessions from being started for closed days
- **SC-TMR-007**: System supports at least 100 concurrent timer sessions without performance degradation
- **SC-TMR-008**: System maintains 99.9% data integrity for timer sessions and intervals
