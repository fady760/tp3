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

---

## Workshop 3 — Provider & Scalable State Management with TDD

- Agent used: Antigravity (AI coding agent).
- Goal: Move from setState() to Provider for scalable state management, and add a "Next Client" feature using TDD (Red → Green → Refactor).
- Topics discussed:
  - Limitations of setState(): prop-drilling and unnecessary subtree rebuilds.
  - How ChangeNotifier + notifyListeners() turn a plain model into a listenable one.
  - Difference between context.watch<T>() (rebuild on change) and context.read<T>() (call methods without rebuilding).
  - Wrapping the app in ChangeNotifierProvider at the root so descendants can access the provider via BuildContext.
  - Converting WaitingRoomScreen from StatefulWidget to StatelessWidget since the provider owns the state.
  - Using Key('nextClientButton') to find a widget reliably in widget tests.
  - Writing the widget test with ChangeNotifierProvider wrapping WaitingRoomApp because the app no longer creates the provider internally.
- Corrected bugs in the assignment's printed code:
  - nextClient() guard was inverted in the PDF (isEmpty); corrected to isNotEmpty so the first client is actually removed.
  - Add button's guard was inverted in the PDF (isEmpty); corrected to isNotEmpty so clients are actually added.
- Challenges encountered:
  - Widget test failing after refactor because the test needed to wrap WaitingRoomApp in ChangeNotifierProvider itself (the app no longer owns the provider).
  - Understanding that context.watch rebuilds the whole WaitingRoomScreen widget, while context.read does not — so watch is used once at the top, read for method calls.
  - Renaming WaitingRoomManager → QueueProvider required updating every import and every test reference.
- Outcome: All unit and widget tests pass. App has add, remove, and next-client features powered by Provider.
