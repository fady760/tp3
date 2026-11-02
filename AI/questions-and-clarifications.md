# Questions and Clarifications

**1. What exactly is the "Red → Green → Refactor" cycle in TDD, and why does the workshop insist the first test must fail?**
The cycle is: write a failing test (Red), write minimal code to make it pass (Green), then clean up (Refactor). The test must fail first to prove it actually tests something — a test that passes immediately could be a false positive.

**2. Should I write the WaitingRoomManager class first or the test first? The TP says test first — why does that matter in practice?**
Test first. It forces you to define the expected behavior (the API: class name, method names, return values) before implementation, which leads to cleaner design and guarantees the test is meaningful.

**3. What is the difference between a unit test and a widget test in Flutter, and why do we need both for this app?**
Unit tests test pure logic (like WaitingRoomManager) without any UI. Widget tests render widgets and simulate user interaction. We need both: the unit test proves the queue logic works, and the widget test proves the UI is wired to that logic correctly.

**4. Why does the test import package:waiting_room_app/waiting_room_manager.dart instead of using a relative path?**
Using the package import matches how the app itself imports files and ensures the test resolves the same file, avoiding subtle path/duplication issues.

**5. What does the clients getter do, and why is _clients private while clients is public?**
_clients is the internal mutable list. The public clients getter exposes a read-only view so the UI can read the list without being able to mutate it directly — encapsulation.

**6. Why does addClient return void instead of returning a new list?**
The method mutates internal state; the UI reads via the clients getter. Returning a list would create two sources of truth and complicate the API.

**7. I ran flutter test and it failed with "class WaitingRoomManager isn't defined" — is that the expected "Red" state?**
Yes. That is exactly the Red phase: the test describes intent, and the failing compile confirms the test is real and the code does not exist yet.

**8. What is StatefulWidget and how is it different from StatelessWidget in this app?**
StatelessWidget is immutable (WaitingRoomApp just builds MaterialApp). StatefulWidget holds mutable state that can change over time (WaitingRoomScreen), triggering rebuilds when setState() is called.

**9. Why do we need both WaitingRoomScreen and _WaitingRoomScreenState? Why is the state class private?**
The widget is the configuration; the State object holds mutable data (the manager, the controller) and the build method. The state class is private because nothing outside the file should touch it.

**10. Why is _manager declared as final WaitingRoomManager _manager = WaitingRoomManager(); inside the state — won't setState() replace it?**
No. final only prevents reassigning the reference. setState() does not replace the manager; it just tells Flutter to rebuild, and the same manager instance is used with its updated internal list.

**11. What does setState() actually do under the hood, and why must the addClient call be inside it?**
setState() marks the widget as dirty and schedules a rebuild. Mutating state without setState() means Flutter doesn't know to rebuild, so the UI stays stale.

**12. Why do we need a TextEditingController, and why call _controller.clear() after adding?**
The controller holds the current text value of the TextField. Clearing it after adding resets the field so the user can type the next name without manually deleting.

**13. Why is the check if (_controller.text.isNotEmpty) there — does the test require it?**
The provided tests don't strictly require it, but it prevents adding empty client names if a user taps Add with an empty field — a reasonable guard for the UI.

**14. In the widget test, why await tester.pumpWidget(const WaitingRoomApp()) before enterText?**
pumpWidget builds the widget tree so the finders (TextField, ElevatedButton) can actually locate widgets. Without it there is nothing to interact with.

**15. What is the difference between pump() and pumpAndSettle(), and why does the TP use pump()?**
pump() advances one frame. pumpAndSettle() keeps pumping until no more frames are scheduled (e.g. animations). setState() schedules one rebuild, so a single pump() is enough here.

**16. Why does find.byType(TextField) work when there's only one TextField — what happens if we add a second one?**
find.byType matches all widgets of that type. With two TextFields, the finder would match two widgets and tester.enterText would throw because it expects exactly one — you'd need to scope it with find.descendant or a Key.

**17. Why does the TP use ListView.builder instead of a simple Column of Cards?**
ListView.builder lazily builds only visible items, which scales well for long lists; Column builds everything eagerly. It's also the idiomatic pattern for dynamic lists.

**18. What is the role of Expanded around the TextField and around the ListView?**
Expanded tells the child to take the remaining space in a Row/Column. Around TextField it lets it fill horizontal space next to the Add button; around ListView it gives the list the remaining vertical space in the Column.

**19. When I add the remove feature, why does the unit test use removeClient('John Doe') and then check clients.first == 'Jane Doe'?**
It verifies both the removal (length 1) and that the correct element was removed (the remaining one is Jane, not John). Checking only length would miss a bug where the wrong client was removed.

**20. In the widget remove test, why do we need to enterText and tap the Add button first instead of rendering a client directly?**
The widget test exercises the real user flow: type, add, then delete. It also guarantees the delete icon is rendered as part of the normal UI path.

**21. find.byIcon(Icons.delete) — what if the list is empty and no delete icon exists? What error should I expect?**
The finder would match zero widgets and tester.tap would throw a "found 0 widgets" error. That's why the test adds "Bob" first.

**22. Why does _manager.removeClient(clientName) need to be inside setState() in the IconButton onPressed?**
Because it mutates state; wrapping it in setState() is what triggers the rebuild that removes the tile and updates the "Clients in Queue" text.

**23. The widget test expects find.text('Clients in Queue: 0') after removal — how does that string interpolation update automatically?**
Because the Text widget is built with 'Clients in Queue: ${_manager.clients.length}'. On rebuild (via setState), the interpolation is re-evaluated with the new length, so the text updates.

**24. I noticed WaitingRoomScreen() is passed without const in the TP — is that intentional? Does it affect tests?**
It's a minor style issue; WaitingRoomScreen has a const constructor but the call could be const WaitingRoomScreen(). It does not affect test behavior.

**25. If I wanted to add a "clear all" button, what would the TDD order be (tests first, then UI)?**
Write a unit test for a clearClients() method → implement it → write a widget test for a "Clear All" button → add the button in the UI. Same Red → Green → Refactor cycle.

**26. Why does the workshop say "no extra functionality" in the Green step — isn't it better to write the full feature at once?**
No. Minimal code in Green keeps the test as the sole driver of behavior and prevents untested code from creeping in. Extra features get their own Red test first.

**27. What's the purpose of the AI/ folder deliverable, and what should the summary contain?**
It documents the AI-assisted work: which agent was used, topics discussed, challenges, outcome. It serves as a record of the reasoning process and proves the work was done hands-on.

**28. Which skills did I actually practice in this TP (state management, layout, testing), and how do they connect to the next workshop (Provider + Supabase)?**
StatefulWidget + setState() for local state; layout with Column, Row, Expanded, ListView; unit and widget testing with TDD. Next, Provider introduces shared state management across widgets, and Supabase adds persistence — both build directly on the patterns practiced here.
