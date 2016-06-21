---
layout: post
title:  A(nother) code snippet for custom roles in Loopback framework
date:   2016-06-21 19:20:00 +0530
author: Arun
categories: loopback
---
[Loopback framework](https://strongloop.com/node-js/loopback-framework/) provides powerful features for controlling access to data models using authorization, access control lists (ACL) and Roles. Though I
found it little bit difficult to begin with, the efforts did pay off recently for one of my hobby projects. I am of the opinion that it's better to not re-invent
 the wheel and leave intricacies to frameworks which is why they exist in the first place.

For access control, the loopback framework provides static, dynamic and custom roles. Custom Roles, which this post is about, are created using a boot script.
The [Loopback documentation](https://docs.strongloop.com/display/public/LB/Defining+and+using+roles) in addition to explaining these concepts also has a link
to a github repo where you can see an example. In the scenario used for the example, A `Project` "belongsTo" an owner of type `user` (extended from User).
 There is an entity called `Team` "hasMany" `users`. The project should be accessible to all users
 who are in the same team as the owner of the project. While I was going through the example, I had wished for another example but couldn't locate one.
 That inspired me to come up with this post.

### My requirement
My Expense Vouchers project had a few requirements which made ACL/Roles very useful. The project has the
following data models:

  * Organization
  * Employees (extended from the User base model)
  * Vouchers

An Organization model "hasMany" Employees. An Employee "hasMany" Vouchers. An Organization "hasMany"
Vouchers. A Voucher "belongsTo" an Employee.

An Employee should be able to create vouchers in an
organization. He gets read/write access to his own vouchers but not to the ones that belong to other
employees (even if they are in the same organization). Some employees are marked as Managers using an
isManager boolean field in the model. A Manager has read/write access to all vouchers in his organization.
Pretty simple requirements, I'd say.

### The solution
For my satisfying my requirements, I used the $owner dynamic role provided by Loopback so that an employee
can edit and view only his vouchers. For employees with managerial privileges - aka read/write access to all vouchers in an organization,
I created a custom role called "Manager". Here is a gist with the registerResolver function that I used for
defining the Manager role. *(The real purpose of this post)*. The rest of the story is in the code.

<script src="https://gist.github.com/arunkutty/86dc5dbb36f7f1cf402594330c0b26b7.js"></script>
