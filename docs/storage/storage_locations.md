title: Storage Locations

The storage on Katana is split into several different types, each of which serves a different purpose. 

!!! important
    We have just said *each of which serves a different purpose*. Despite that, there will be overlap. And personal preference. In most cases it will be obvious where to put your information. If it isn't and you need help with your decision making, you can email the [Research Data team](https://research.unsw.edu.au/research-data-management-unsw>) team at rdm@unsw.edu.au for advice. They are friendly people.

    If you specifically wish to increase Katana storage allocations, please email the [IT Service Centre](mailto:ITServiceCentre@unsw.edu.au) - note that such increases are not automatic.

## Storage  Summary

<table>
<thead>
  <tr>
    <th>Storage type</th>
    <th>Location</th>
    <th>Alias</th>
    <th>Purpose</th>
    <th>Backed up?</th>
    <th>Size limit</th>
    <th>Who has access</th>
	<th>Where can it be used?</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Home drive</td>
    <td>/home/z1234567</td>
    <td>$HOME</td>
    <td>Source code and programs</td>
    <td>Y</td>
    <td>10Gb</td>
    <td>Only the user</td>
	<td>Anywhere</td>
  </tr>
  <tr>
    <td>User scratch</td>
    <td>/srv/scratch/z1234567</td>
    <td>/srv/scratch/$USER</td>
    <td>Data files</td>
    <td>N</td>
    <td>128 GB</td>
    <td>Only the user</td>
	<td>Anywhere</td>
  </tr>
  <tr>
    <td>Shared scratch</td>
    <td>/srv/scratch/group_name</td>
    <td>/srv/scratch/group_name</td>
    <td>Data files and programs shared by a team</td>
    <td>N</td>
    <td>Upon group requirements</td>
    <td>All users in the group</td>
	<td>Anywhere</td>
  </tr>
  <tr>
    <td>Local scratch</td>
    <td>Intialised upon job running</td>
    <td>$TMPDIR</td>
    <td>Faster job completion with large datasets and temp files</td>
    <td>N</td>
    <td>200Gb shared between node users</td>
    <td>Temporary for each job</td>
	<td>Only on compute nodes</td>
  </tr>
  <tr>
    <td>UNSW Research Storage</td>
    <td>/home/z1234567/sharename</td>
    <td>$HOME/sharename</td>
    <td>Storage of shared user and data files</td>
    <td>Y</td>
    <td colspan="2">Depends on storage location</td>
	<td>Only on KDM (and login nodes)</td>
  </tr>
</tbody>
</table>