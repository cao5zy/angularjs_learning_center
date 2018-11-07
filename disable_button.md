# disable button

## Disable button my condition
```
<button class="btn btn-warning btn-sm" type="button" [disabled]="!isNotOwner(member)" (click)="removeMember(obj.project, member)">           
    <span *ngIf="isNotOwner(member)" class="glyphicon glyphicon glyphicon-remove" aria-hidden="true"></span>&nbsp;&nbsp; {{ member }}          
</button>
```

## Reference
- https://stackoverflow.com/questions/35535350/angular2-disable-button
