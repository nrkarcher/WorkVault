Bug busters and Hell intersections working on 11.0
- The teams have grabbed their 11.0 work already
Verify.h what to do moving forward
- Lucius put up a stack overflow post
- If we upgrade to QT 6.15 we can get additional QVERIFY functionality
- If not we are stuck with verify.h
- Verify is easier to maintain long term. QVerify has slightly better functionality.
- Mo: You can always QVERIFY(VerifyThrows()) and return bool from VerifyThrows and that will solve the locating problem
- Team voted to keep Verify.h
Ticket for documentation has been converted to a lolla initiative.
- This will get pushed to next year? (If it's a Lolla project)
- This seems like needed functionality - Should it be added to the tech debt?
	- Aidan - This is important and should not be pushed off
- We have multiple places to put notes and dev Q&A 
- Mo and Aidan to add ticket to 2024 tech debt for documentation
- Timebox: Assign points in grooming
QT 6.7 adds support for 2nd bar, line and scatter graphs
- If we update QT we get these features.
- Might be able to replace IOcomp
- Do we want a pros/cons list for QT upgrade?
- Currently we have a ticket to upgrade to 5.15
- Research what we can gain from upgrading to 6.7 Garby (6 Month timeline)

Retro

How do we address tickets that blow up like the wellzone ticket? What can we do?
- Should this ticket be re-estimated after completion?
- Bug Busters - Discuss re-estimating the ticket.
- It seems like the team was doing the right thing at the time. Maybe push a little harder? Keep an eye on scope creep.
- 



