# Testing

> Treat bugs in new features and regressions in existing features differently. If a bug surfaces during development, take the time to understand the mistake, fix it, and move on. **If a regression appears (i.e., something worked before but doesn't anymore), then it's likely to reappear. Create an automated test to protect against that regression in the future**.

QA brings an important perspective to the development of a feature, and good QA engineers know where bugs usually hide and can advise developers on probable "gotchas".

But isn't exploratory testing manual testing? Nope. At least not in the same sense as manual regression testing. Exploratory testing is a risk-based, critical thinking approach to testing that enables the person testing to use their knowledge of risks, implementation details, and the customers' needs. Knowing these things earlier in the testing process allows the developer or QA engineer to find issues rapidly and comprehensively, without the need for scripted test cases, detailed test plans, or requirements. We find it's much more effective than traditional manual testing, because we can take insights from exploratory testing sessions back to the original code and automated tests. Exploratory testing also teaches us about the experience of using the feature in a way that scripted testing doesn't.

Maintaining quality involves a blend of exploratory and automated testing. As new features are developed, exploratory testing ensures that new code meets the quality standard in a broader sense than automated tests alone. This includes ease of use, pleasing visual design, and overall usefulness of the feature in addition to the robust protections against regressions that automated testing provides. 

# References

- [Testing](https://www.atlassian.com/agile/software-development/testing)

# Related link

- [Quality at Speed, How JIRA Does QA - Webinar](https://www.youtube.com/watch?v=yRP29wFqu20&t=7s)
