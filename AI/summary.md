# Summary of AI-Assisted Discussions

## Agent Used
- **Agent:** Antigravity (AI coding agent)
- **Project:** Workshop 2 – State Management Basics + TDD
- **App:** Local Waiting Room App (Flutter)

## Objective
Build an interactive Flutter app that lets a user add and remove clients from a waiting room queue, using Test-Driven Development (Red → Green → Refactor), StatefulWidget, setState(), and both unit and widget tests.

## Topics Discussed
- Understanding the TDD cycle (Red, Green, Refactor) and why the test must fail first
- Difference between unit tests (test()) and widget tests (testWidgets(), WidgetTester, pumpWidget, enterText, tap, pump)
- Using StatefulWidget + setState() to rebuild the UI on state change
- Role of TextEditingController for the input field
- Layout widgets: Column, Row, Expanded, SizedBox, ListView.builder, Card, ListTile
- Adding the remove feature via IconButton + Icons.delete in the ListTile trailing slot
- Common pitfalls: forgetting setState(), forgetting await tester.pump() after a state change, empty input validation

## Outcome
All tests pass. The app supports adding and removing clients with a dynamically updating queue count.

## Challenges Encountered
1. First widget test failed until I realized I had to call await tester.pump() after tapping the button so the UI would rebuild.
2. Understanding why the private _WaitingRoomScreenState class is needed separately from WaitingRoomScreen (the widget vs. its state).
3. Remembering to delete the default lib/main.dart before starting from scratch, as the TP instructs.

## Next Steps (per the workshop)
- Advanced state management with Provider
- Persistence with Supabase
- (These are covered in future workshops, not this TP.)
